# ArmPkg Debugging

## Backgound

### EFI binary format

EFI binary files are not ELF formated files. It means most ARM debugger
tools will not understand this format. But an EFI binary file is
generated from an EFL binary by the EDK2 BaseTools. The original EFL
file can be found under
\[EDK2_ROOT\]/Build/\[YOUR_PROJECT\]/DEBUG\_\[YOUR_TOOLCHAIN\]/ARM/.../.../DEBUG/\[YOUR_EFI_MODULE\].dll
Example:

    Build/ArmRealViewEb-RTSM-A8/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/Sec/Sec/DEBUG/ArmPlatformSec.dll

The .dll extension is not relevent. These files are really ELF files.
The conversion from ELF to PE/COFF will mainly modify the file header
(and its size).

#### Topology of the UEFI Firmware File

A UEFI Firmware File (actually a UEFI Firmware Device - FD file) is a
collection of UEFI binaries encapsulated into a single image. The format
of this image is defined by the Platform Initialization Specification
Volume 3. A Vector Table is located at the base of this file. A 'BL'
branch instruction at the base of the firwmare (location of the Reset
Entry into the Vector Table) will jump to the first 'SEC' module of the
UEFI Firmware Image.

### Hardware Debugger & Printed Debug Command lines

The hardware debugger cannot read the command lines printed into the
serial console. These command lines are only there to help the firmware
engineer to load the symbols for the corresponding modules into his/her
debugger tool.

## Debugging code when the 'Debug Command lines' are printed into the serial console

1\. Stop your target (eg: with a hardware debugger)

2\. Identify the value of the 'PC' (Program Counter Register)

3\. Use the printed debug command lines to identify in which module your
'PC' is located. Example:

    Boot firmware (version  built at 17:48:18 on Aug 21 2012)
add-symbol-file
/work/dev/uefi/edk2/Build/ArmVExpress-CTA9x4-Standalone/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/Sec/Sec/DEBUG/ArmPlatformSec.dll
0x44000180
    Trust Zone Configuration is disabled
add-symbol-file
/work/dev/uefi/edk2/Build/ArmVExpress-CTA9x4-Standalone/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/PrePeiCore/PrePeiCoreMPCore/DEBUG/ArmPlatformPrePeiCore.dll
0x45000180
add-symbol-file
/work/dev/uefi/edk2/Build/ArmVExpress-CTA9x4-Standalone/DEBUG_RVCTLINUX/ARM/MdeModulePkg/Core/Pei/PeiMain/DEBUG/PeiCore.dll
0x450049CC
    (...)

If your PC register is equal to 0x45000248 then your CPU is running code
from the 'PrePeiCore' module.

4\. Load the symbols by copying and pasting the command line into your
debugger.

add-symbol-file
/work/dev/uefi/edk2/Build/ArmVExpress-CTA9x4-Standalone/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/PrePeiCore/PrePeiCoreMPCore/DEBUG/ArmPlatformPrePeiCore.dll
0x45000180

### Debugging initial bring-up code (SEC phase)

Scope: The UEFI Debug Support has not been initialized yet. And
the platform crashes before the UART or any output are available to the
firmware engineer.

As you do not have UART yet to print out to the location of the initial
symbols, you need to calculate manually the corresponding ELF base
address that will be used to load the symbols at the correct location.
We will use the base address of the first called function as a
reference. 1. Identify the first function following your EDK2 boot flow.
If your EDK2 firmware crashes in - the Sec module, then the first
function is \_ModuleEntryPoint of \[EDK2_ROOT\]/ArmPlatformPkg/Sec/ -
the PrePi module, then the first function is \_ModuleEntryPoint of
\[EDK2_ROOT\]/ArmPlatformPkg/PrePi/

2\. Retrieve the base address of this symbol in the corresponding ELF
file. Example:

$ fromelf -s Build/ArmRealViewEb-RTSM-A8/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/Sec/Sec/DEBUG/ArmPlatformSec.dll | grep
_ModuleEntryPoint
      1217  _ModuleEntryPoint          0x00000004   Gb    1  Code  De
    Or:


$ arm-linux-gnueabi-objdump -t
Build/ArmRealViewEb-RTSM-A8/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/Sec/Sec/DEBUG/ArmPlatformSec.dll | grep_ModuleEntryPoint
    00000004 g     F ER_RO  000000c8 .hidden_ModuleEntryPoint

3\. Get the address of \_ModuleEntryPoint in the UEFI Firmware file. The
first 32bit double word of your firmware should be a 'BL' jump to this
entry point.

    S:0xEB00005F : BL       {pc}+0x184 ; 0x184

Note: 'BL' is a branch relative to the Program Counter (PC) register. In
the case of the example, the absolute address of \_ModuleEntryPoint will
be: *0x0 (base address) + 0x184 (offset from the base address) =
0x184*

4\. Calculate the translated ELF base address for this first module:
*Absolute Address of \_ModuleEntryPoint on the Target - Absolute
Address of \_ModuleEntryPoint in the ELF file = 0x184 - 0x004 =
0x180*

5\. Load the symbols for this module:

    ```text
    add-symbol-file
    [EDK2_ROOT]/Build/ArmRealViewEb-RTSM-A8/DEBUG_RVCTLINUX/ARM/ArmPlatformPkg/Sec/Sec/DEBUG/ArmPlatformSec.dll 0x180
    ```
