# Introduction

The memory profile feature was introduced to help a developer analyze the hardware memory reservation in a UEFI firmware
implementation. After enhanced, the memory profile feature can be also used for memory leak detection. The enhanced
memory profile feature supports

1. User can know which line of code calls gBS->AllocateXXX() / gSmst->SmmAllocateXXX().
2. User can know which line of code calls AllocateXXX() API of MemoryAllocationLib that will call gBS->AllocateXXX() /
gSmst->SmmAllocateXXX().
3. User can know which line of code calls a specific API that will call gBS->AllocateXXX() / gSmst->SmmAllocateXXX() or
AllocateXXX() API of MemoryAllocationLib.
4. User can know total memory allocated by a specific line of code.
5. User can configure to record single module only.
6. User can configure when to enable recording.
7. User can know RVA<->Symbol (Function, Source, Line).

This wiki page only focuses on how to enable memory leak detection with memory profile feature. The part I and II of
white paper
"A_Tour_Beyond_BIOS_Implementing_Profiling_in_EDKII"([https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_Profiling_in_EDK_II.pdf](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_Profiling_in_EDK_II.pdf))
can be referred if you really want to know the inside working mechanism of memory profile feature.

## PCDs, libraries and tools

### PCDs

There are three PCDs PcdMemoryProfilePropertyMask, PcdMemoryProfileMemoryType and PcdMemoryProfileDriverPath defined in
MdeModulePkg.dec([https://github.com/tianocore/edk2/blob/master/MdeModulePkg/MdeModulePkg.dec](https://github.com/tianocore/edk2/blob/master/MdeModulePkg/MdeModulePkg.dec))
to control the behaviors of memory profile feature.

```
## The mask is used to control memory profile behavior.    
##  BIT0 - Enable UEFI memory profile.  
##  BIT1 - Enable SMRAM profile.  
##  BIT7 - Disable recording at the start.  
## @Prompt Memory Profile Property.
## @Expression  0x80000002 | (gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfilePropertyMask & 0x7C) == 0
gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfilePropertyMask|0x0|UINT8|0x30001041

## This flag is to control which memory types of alloc info will be recorded by DxeCore & SmmCore.    
## For SmmCore, only EfiRuntimeServicesCode and EfiRuntimeServicesData are valid.  
#
## Below is bit mask for this PCD: (Order is same as UEFI spec)
## EfiReservedMemoryType 0x0001
## EfiLoaderCode 0x0002
## EfiLoaderData 0x0004
## EfiBootServicesCode 0x0008
## EfiBootServicesData 0x0010
## EfiRuntimeServicesCode 0x0020
## EfiRuntimeServicesData 0x0040
## EfiConventionalMemory 0x0080
## EfiUnusableMemory 0x0100
## EfiACPIReclaimMemory 0x0200
## EfiACPIMemoryNVS 0x0400
## EfiMemoryMappedIO 0x0800
## EfiMemoryMappedIOPortSpace 0x1000
## EfiPalCode 0x2000
## EfiPersistentMemory 0x4000
## OEM Reserved 0x4000000000000000
## OS Reserved 0x8000000000000000
#
## e.g. Reserved+ACPINvs+ACPIReclaim+RuntimeCode+RuntimeData are needed, 0x661 should be used.  
#
## @Prompt Memory profile memory type.
gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfileMemoryType|0x0|UINT64|0x30001042

## This PCD is to control which drivers need memory profile data.    
## For example:  
## One image only (Shell):  
##     Header                    GUID  
##     {0x04, 0x06, 0x14, 0x00,  0x83, 0xA5, 0x04, 0x7C, 0x3E, 0x9E, 0x1C, 0x4F, 0xAD, 0x65, 0xE0, 0x52, 0x68, 0xD0, 0xB4, 0xD1,  
##      0x7F, 0xFF, 0x04, 0x00}  
## Two or more images (Shell + WinNtSimpleFileSystem):  
##     {0x04, 0x06, 0x14, 0x00,  0x83, 0xA5, 0x04, 0x7C, 0x3E, 0x9E, 0x1C, 0x4F, 0xAD, 0x65, 0xE0, 0x52, 0x68, 0xD0, 0xB4, 0xD1,  
##      0x7F, 0x01, 0x04, 0x00,  
##      0x04, 0x06, 0x14, 0x00,  0x8B, 0xE1, 0x25, 0x9C, 0xBA, 0x76, 0xDA, 0x43, 0xA1, 0x32, 0xDB, 0xB0, 0x99, 0x7C, 0xEF, 0xEF,  
##      0x7F, 0xFF, 0x04, 0x00}  
## @Prompt Memory profile driver path.
gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfileDriverPath|{0x0}|VOID*|0x00001043
```

### Libraries

After memory profile feature is enabled by configuring PCDs above, DxeCore and SmmCore will record alloc info for the
code calls gBS->AllocateXXX() / gSmst->SmmAllocateXXX() by default. In order to record alloc info for the code calls
AllocateXXX() API of MemoryAllocationLib or a specific API, MemoryProfileLib library
class([https://github.com/tianocore/edk2/blob/master/MdeModulePkg/Include/Library/MemoryProfileLib.h](https://github.com/tianocore/edk2/blob/master/MdeModulePkg/Include/Library/MemoryProfileLib.h))
and instances are defined and implemented in MdeModulePkg.

* For DXE_CORE:

```
MemoryAllocationLib|MdeModulePkg/Library/DxeCoreMemoryAllocationLib/DxeCoreMemoryAllocationProfileLib.inf
MemoryProfileLib|MdeModulePkg/Library/DxeCoreMemoryAllocationLib/DxeCoreMemoryAllocationProfileLib.inf
```

* For DXE_DRIVER / DXE_RUNTIME_DRIVER / UEFI_DRIVER / UEFI_APPLICATION:

```
MemoryAllocationLib|MdeModulePkg/Library/UefiMemoryAllocationProfileLib/UefiMemoryAllocationProfileLib.inf
MemoryProfileLib|MdeModulePkg/Library/UefiMemoryAllocationProfileLib/UefiMemoryAllocationProfileLib.inf
```

* For SMM_CORE:

```
MemoryAllocationLib|MdeModulePkg/Library/PiSmmCoreMemoryAllocationLib/PiSmmCoreMemoryAllocationProfileLib.inf
MemoryProfileLib|MdeModulePkg/Library/PiSmmCoreMemoryAllocationLib/PiSmmCoreMemoryAllocationProfileLib.inf
```

* For DXE_SMM_DRIVER:

```
MemoryAllocationLib|MdeModulePkg/Library/SmmMemoryAllocationProfileLib/SmmMemoryAllocationProfileLib.inf
MemoryProfileLib|MdeModulePkg/Library/SmmMemoryAllocationProfileLib/SmmMemoryAllocationProfileLib.inf
```

After correct MemoryAllocationLib and MemoryProfileLib are linked, the alloc info for the code calls AllocateXXX() API
of MemoryAllocationLib will be recorded. If you want to record the alloc info for the code calls a specific API, the
specific API needs to be updated to call MemoryProfileLibRecord() API of MemoryProfileLib.

### Tools

After memory profile feature is enabled and the system boots to SHELL,
MemoryProfileInfo([https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Application/MemoryProfileInfo](https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Application/MemoryProfileInfo))
application in MdeModulePkg can be used to dump the profile information, then
MemoryProfileSymbolGen.py([https://github.com/tianocore/edk2/blob/master/BaseTools/Scripts/MemoryProfileSymbolGen.py](https://github.com/tianocore/edk2/blob/master/BaseTools/Scripts/MemoryProfileSymbolGen.py))
can be used to convert RVA address to symbols for the profile information from MemoryProfileInfo application. In Linux,
MemoryProfileSymbolGen.py will parse the debug information generated by the GCC compiler through “nm”. In Windows,
MemoryProfileSymbolGen.py will parse the PDB information generated by the MSVC compiler through Microsoft Visual Studio
“DIA SDK”.

## How to enable

1. **Configure gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfilePropertyMask in platform dsc to enable memory profiling
for UEFI memory or SMRAM.**

  *For example, only enable UEFI memory profiling.*

  ```
  [PcdsFixedAtBuild.common]
    gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfilePropertyMask|0x1
  ```

2. **Configure gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfileMemoryType in platform dsc to enable memory profiling for
specific memory types.**

  *For example, only enable EfiBootServicesData memory profiling.*

  ```
  [PcdsFixedAtBuild.common]
    gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfileMemoryType|0x10
  ```

3. **Configure gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfileDriverPath in platform dsc to only enable profiling for
some specific drivers.**

  *For example, only enable memory profiling for SHELL.*

  ```
  [PcdsFixedAtBuild.common]
gEfiMdeModulePkgTokenSpaceGuid.PcdMemoryProfileDriverPath|{0x04, 0x06, 0x14, 0x00, 0x83, 0xA5, 0x04, 0x7C, 0x3E, 0x9E,
0x1C, 0x4F, 0xAD, 0x65, 0xE0, 0x52, 0x68, 0xD0, 0xB4, 0xD1, 0x7F, 0xFF, 0x04, 0x00}
  ```

4. **Link correct MemoryAllocationLib and MemoryProfileLib library instances in platform dsc.**

  *For example, link the library instances for SHELL.*

  ```
  [LibraryClasses.common.UEFI_APPLICATION]
    MemoryAllocationLib|MdeModulePkg/Library/UefiMemoryAllocationProfileLib/UefiMemoryAllocationProfileLib.inf
    MemoryProfileLib|MdeModulePkg/Library/UefiMemoryAllocationProfileLib/UefiMemoryAllocationProfileLib.inf
  ```

5. **\[OPTIONAL\] If you want to record the alloc info for the code calls a specific API, the specific API needs to be
updated to call MemoryProfileLibRecord() API of MemoryProfileLib.**

*For example, you want to record the alloc info for the code calls StrnCatGrow(), the code change needed will like
below.*

  ```c
  diff --git a/ShellPkg/Library/UefiShellLib/UefiShellLib.c b/ShellPkg/Library/UefiShellLib/UefiShellLib.c
  index 35a1a7169c8b..5356305dba21 100644
  --- a/ShellPkg/Library/UefiShellLib/UefiShellLib.c
  +++ b/ShellPkg/Library/UefiShellLib/UefiShellLib.c
  @@ -18,6 +18,9 @@
   #include <Library/SortLib.h>
   #include <Library/BaseLib.h>

  +#include <Library/MemoryProfileLib.h>
+#define ModuleProfileActionStrnCatGrow (MEMORY_PROFILE_ACTION_USER_DEFINED_MASK | MemoryProfileActionAllocatePool |
0x10000)
  +
   #define FIND_XXXXX_FILE_BUFFER_SIZE (SIZE_OF_EFI_FILE_INFO + MAX_FILE_NAME_LEN)

   //
  @@ -3264,11 +3267,31 @@ StrnCatGrow (
           NewSize += 2 * Count * sizeof(CHAR16);
         }
         *Destination = ReallocatePool(*CurrentSize, NewSize, *Destination);
  +      if (*Destination != NULL) {
  +        MemoryProfileLibRecord (
  +          (PHYSICAL_ADDRESS) (UINTN) RETURN_ADDRESS(0),
  +          ModuleProfileActionStrnCatGrow,
  +          EfiBootServicesData,
  +          *Destination,
  +          NewSize,
  +          (CHAR8 *) __FUNCTION__
  +          );
  +      }
         *CurrentSize = NewSize;
       }
     } else {
       NewSize = (Count+1)*sizeof(CHAR16);
       *Destination = AllocateZeroPool(NewSize);
  +    if (*Destination != NULL) {
  +      MemoryProfileLibRecord (
  +        (PHYSICAL_ADDRESS) (UINTN) RETURN_ADDRESS(0),
  +        ModuleProfileActionStrnCatGrow,
  +        EfiBootServicesData,
  +        *Destination,
  +        NewSize,
  +        (CHAR8 *) __FUNCTION__
  +        );
  +      }
     }

     //
  diff --git a/ShellPkg/Library/UefiShellLib/UefiShellLib.inf b/ShellPkg/Library/UefiShellLib/UefiShellLib.inf
  index 782649a7a034..b525b51140b8 100644
  --- a/ShellPkg/Library/UefiShellLib/UefiShellLib.inf
  +++ b/ShellPkg/Library/UefiShellLib/UefiShellLib.inf
  @@ -48,6 +48,7 @@ [LibraryClasses]
     UefiLib
     HiiLib
     SortLib
  +  MemoryProfileLib

   [Protocols]
     gEfiSimpleFileSystemProtocolGuid              ## SOMETIMES_CONSUMES
  ```

6. **\[OPTIONAL, only for MSVC build\] Configure build options.**

* Disable compiler optimization to make sure that RETURN_ADDRESS(0) can get the return address of the calling function
  correctly.

  ```
  MSFT: *_*_*_CC_FLAGS = /Od
  ```

* If you want to build release image, debugging information needs to be enabled.

  ```
  MSFT: RELEASE_*_*_DLINK_FLAGS = /DEBUG
  MSFT: RELEASE_*_*_CC_FLAGS = /Zi
  ```

7. **Include MemoryProfileInfo application in platform dsc.**

  *For example, for X64 build.*

  ```
  [Components.X64]
    MdeModulePkg/Application/MemoryProfileInfo/MemoryProfileInfo.inf
  ```

8. **Boot system to shell, run MemoryProfileInfo.efi to dump the profile information.**

  ```
  MemoryProfileInfo.efi >a MemoryProfileInfo1.txt
  ```

  *The summary data entries in MemoryProfileInfo1.txt will like below.*

  ```
  Summary Data:

Driver - Shell (Usage - 0x0000B606) (Pdb -
f:\git\edk2git\Build\NT32IA32\DEBUG_VS2015x86\IA32\ShellPkg\Application\Shell\Shell\DEBUG\Shell.pdb)
  Caller List:
    Count            Size                   RVA              Action
  ==========  ==================     ================== (================================)
  0x0000011A  0x000000000000B606 <== 0x00000000000152E8 (gBS->AllocatePool)
  ...
  0x00000008  0x0000000000000154 <== 0x0000000000019691 (Lib:ReallocatePool)
  0x00000001  0x0000000000000044 <== 0x000000000001D4EA (StrnCatGrow)
  ...
  0x00000004  0x0000000000000062 <== 0x00000000000196C4 (Lib:AllocateZeroPool)
  0x00000003  0x0000000000000036 <== 0x000000000001E0F9 (StrnCatGrow)
  ...
  0x00000006  0x00000000000000A8 <== 0x0000000000007406 (StrnCatGrow)
  ...
  0x00000001  0x000000000000002C <== 0x0000000000018372 (StrnCatGrow)
  ...
  0x00000001  0x0000000000000068 <== 0x00000000000076F9 (StrnCatGrow)
  ```

9. **Copy MemoryProfileInfo1.txt to OS, run MemoryProfileSymbolGen.py to convert RVA address to symbols for the profile
information.**

Note: In Windows, MemoryProfileSymbolGen.py will parse the PDB information generated by the MSVC compiler through
Microsoft Visual Studio “DIA SDK”, if you have no Dia2Dump.exe, you need to build out Dia2Dump.exe from source(for
example, C:\Program Files (x86)\Microsoft Visual Studio 14.0\DIA SDK\Samples\DIA2Dump).

  ```
  MemoryProfileSymbolGen.py -i MemoryProfileInfo1.txt -o MemoryProfileInfoSymbol1.txt
  ```

  *The summary data entries after converted in MemoryProfileInfoSymbol1.txt will like below.*

  ```
  Summary Data:

Driver - Shell (Usage - 0x0000B606) (Pdb -
f:\git\edk2git\Build\NT32IA32\DEBUG_VS2015x86\IA32\ShellPkg\Application\Shell\Shell\DEBUG\Shell.pdb)
  Caller List:
    Count            Size                   RVA              Action
  ==========  ==================     ================== (================================)
0x0000011A 0x000000000000B606 <== 0x00000000000152E8 (gBS->AllocatePool) (InternalAllocatePool() -
f:\git\edk2git\edk2\mdemodulepkg\library\uefimemoryallocationprofilelib\memoryallocationlib.c:458)
  ...
0x00000008 0x0000000000000154 <== 0x0000000000019691 (Lib:ReallocatePool) (StrnCatGrow() -
f:\git\edk2git\edk2\shellpkg\library\uefishelllib\uefishelllib.c:3269)
0x00000001 0x0000000000000044 <== 0x000000000001D4EA (StrnCatGrow) (ShellCommandRegisterCommandName() -
f:\git\edk2git\edk2\shellpkg\library\uefishellcommandlib\uefishellcommandlib.c:571)
  ...
0x00000004 0x0000000000000062 <== 0x00000000000196C4 (Lib:AllocateZeroPool) (StrnCatGrow() -
f:\git\edk2git\edk2\shellpkg\library\uefishelllib\uefishelllib.c:3284)
0x00000003 0x0000000000000036 <== 0x000000000001E0F9 (StrnCatGrow) (ConvertEfiFileProtocolToShellHandle() -
f:\git\edk2git\edk2\shellpkg\library\uefishellcommandlib\uefishellcommandlib.c:1550)
  ...
0x00000006 0x00000000000000A8 <== 0x0000000000007406 (StrnCatGrow) (EfiShellGetMapFromDevicePath() -
f:\git\edk2git\edk2\shellpkg\application\shell\shellprotocol.c:322)
  ...
0x00000001 0x000000000000002C <== 0x0000000000018372 (StrnCatGrow) (ShellFindFilePath() -
f:\git\edk2git\edk2\shellpkg\library\uefishelllib\uefishelllib.c:1716)
  ...
0x00000001 0x0000000000000068 <== 0x00000000000076F9 (StrnCatGrow) (EfiShellGetFilePathFromDevicePath() -
f:\git\edk2git\edk2\shellpkg\application\shell\shellprotocol.c:484)
  ```

10. **Run some test, and then re-run step 8 and 9 to get MemoryProfileInfo2.txt and MemoryProfileInfoSymbol2.txt.
Compare MemoryProfileInfoSymbol1.txt and MemoryProfileInfoSymbol2.txt to see if there is any difference. If
MemoryProfileInfoSymbol2.txt shows more memory allocated, that means it is a potential memory leak.**

NOTE: We call it is *POTENTIAL* memory leak because it might be legal in some cases. For example, a SHELL may choose to
cache the recent command, or a variable. This must be investigated case by case.
