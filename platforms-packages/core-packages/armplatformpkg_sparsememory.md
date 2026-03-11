# ArmPlatformPkg SparseMemory

UEFI (and Tianocore) supports non-contiguous system memory regions.

### Adding support

One way to add non-contiguous memory is to declare an additional
'Resource Descriptor' HOB during the PEI phase. Example:

    EFI_RESOURCE_ATTRIBUTE_TYPE   ResourceAttributes;

    ResourceAttributes =
        EFI_RESOURCE_ATTRIBUTE_PRESENT |
        EFI_RESOURCE_ATTRIBUTE_INITIALIZED |
        EFI_RESOURCE_ATTRIBUTE_UNCACHEABLE |
        EFI_RESOURCE_ATTRIBUTE_WRITE_COMBINEABLE |
        EFI_RESOURCE_ATTRIBUTE_WRITE_THROUGH_CACHEABLE |
        EFI_RESOURCE_ATTRIBUTE_WRITE_BACK_CACHEABLE |
        EFI_RESOURCE_ATTRIBUTE_TESTED;

    // Declared the additional DRAM from 2GB to 4GB
    SparseMemoryBase = 0x0880000000;
    SparseMemorySize = SIZE_2GB;

    BuildResourceDescriptorHob (
        EFI_RESOURCE_SYSTEM_MEMORY,
        ResourceAttributes,
        SparseMemoryBase,
        SparseMemorySize);

Do not forget to map this region in the MMU Page Table. Otherwise, when
the DXE phase would try to load the UEFI modules at the top of the
system memory it would crash.

The UEFI code for the Base and Foundation FVP models support the
non-contiguous memory. See the code in
ArmPlatformPkg/ArmVExpressPkg/Library/ArmVExpressLibRTSM/RTSMMem.c.

### Debugging

If the additional memory region has been correctly declared, you should
see the EFI modules loaded in this region:

    (...)
add-symbol-file
/tianocore/Build/ArmVExpress-FVP-AArch64/DEBUG_GCC48/AARCH64/ArmPkg/Drivers/CpuDxe/CpuDxe/DEBUG/ArmCpuDxe.dll
0x8FFFAC800
    Loading driver at 0x008FFFAC000 EntryPoint=0x008FFFAC850 ArmCpuDxe.efi
add-symbol-file
/tianocore/Build/ArmVExpress-FVP-AArch64/DEBUG_GCC48/AARCH64/MdeModulePkg/Core/RuntimeDxe/RuntimeDxe/DEBUG/RuntimeDxe.dll
0x8FFFC6260
    Loading driver at 0x008FFFC6000 EntryPoint=0x008FFFC62B0 RuntimeDxe.efi
add-symbol-file
/tianocore/Build/ArmVExpress-FVP-AArch64/DEBUG_GCC48/AARCH64/MdeModulePkg/Universal/SecurityStubDxe/SecurityStubDxe/DEBUG/SecurityStubDxe.dll
0x8FFFA5260
    Loading driver at 0x008FFFA5000 EntryPoint=0x008FFFA52B0 SecurityStubDxe.efi
    (...)

The system memory is also viewable from the EFI Shell:

    UEFI v2.40 (ARM Fixed Virtual Platform EFI Apr 11 2014 11:55:10, 0x00000000)
    Shell> memmap
    Type      Start            End              #pages             Attributes
    Available 0000000080000000-0000000087FFFFFF 0000000000008000 000000000000000F
    Available 000000008C000000-00000000FFAB0FFF 0000000000073AB1 000000000000000F
    BS_Data   00000000FFAB1000-00000000FFFFFFFF 000000000000054F 000000000000000F
    Available 0000000880000000-00000008FADC2FFF 000000000007ADC3 000000000000000F
    LoaderCode 00000008FADC3000-00000008FAEBAFFF 00000000000000F8 000000000000000F
    BS_Code   00000008FAEBB000-00000008FAFB2FFF 00000000000000F8 000000000000000F
    RT_Code   00000008FAFB3000-00000008FAFBEFFF 000000000000000C 800000000000000F
    RT_Data   00000008FAFBF000-00000008FAFD7FFF 0000000000000019 800000000000000F
    RT_Code   00000008FAFD8000-00000008FAFF4FFF 000000000000001D 800000000000000F
    Available 00000008FAFF5000-00000008FC6DAFFF 00000000000016E6 000000000000000F
    BS_Data   00000008FC6DB000-00000008FC74DFFF 0000000000000073 000000000000000F
    Available 00000008FC74E000-00000008FC7C0FFF 0000000000000073 000000000000000F
    BS_Data   00000008FC7C1000-00000008FC8B8FFF 00000000000000F8 000000000000000F
    Available 00000008FC8B9000-00000008FC8D5FFF 000000000000001D 000000000000000F
    BS_Data   00000008FC8D6000-00000008FC906FFF 0000000000000031 000000000000000F
    Available 00000008FC907000-00000008FC91EFFF 0000000000000018 000000000000000F
    BS_Data   00000008FC91F000-00000008FC95CFFF 000000000000003E 000000000000000F
    Available 00000008FC95D000-00000008FC973FFF 0000000000000017 000000000000000F
    BS_Data   00000008FC974000-00000008FFE28FFF 00000000000034B5 000000000000000F
    Available 00000008FFE29000-00000008FFE91FFF 0000000000000069 000000000000000F
    BS_Code   00000008FFE92000-00000008FFFB8FFF 0000000000000127 000000000000000F
    RT_Code   00000008FFFB9000-00000008FFFCCFFF 0000000000000014 800000000000000F
    RT_Data   00000008FFFCD000-00000008FFFFEFFF 0000000000000032 800000000000000F
    BS_Data   00000008FFFFF000-00000008FFFFFFFF 0000000000000001 000000000000000F
    MMIO      000000000C000000-000000000FFEFFFF 0000000000003FF0 8000000000000001
    MMIO      000000001C170000-000000001C170FFF 0000000000000001 8000000000000001
      Reserved  :          0 Pages (0)
      LoaderCode:        248 Pages (1,015,808)
      LoaderData:          0 Pages (0)
      BS_Code   :        543 Pages (2,224,128)
      BS_Data   :     15,327 Pages (62,779,392)
      RT_Code   :         61 Pages (249,856)
      RT_Data   :         75 Pages (307,200)
      ACPI Recl :          0 Pages (0)
      ACPI NVS  :          0 Pages (0)
      MMIO      :     16,369 Pages (67,047,424)
      Available :  1,015,938 Pages (4,161,282,048)
    Total Memory: 4095 MB (4,294,905,856 Bytes)
    Shell>

To debug these non-contiguous memory regions with ARM DS-5, you would
need to add the new system memory region. Example for the ARM FVP Base
model:

source "ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py" -f (0x88000000,0x04000000) -m (0x80000000,0x80000000) -m
(0x880000000,0x80000000) -a -v

### Passing this non-contiguous memory region to Linux

If the Linux image does not contain a EFI Stub then you would need to
declare this region in the Device Tree. Example:

    ```text
        memory@80000000 {
          device_type = "memory";
          reg = <0x00000000 0x80000000 0x0 0x80000000>,
                <0x00000008 0x80000000 0x1 0x80000000>;
        };

    If the Linux image contains the EFI Stub then Linux figures out by
    itself the amount of memory exposed by UEFI. Example:

        EFI stub: Booting Linux Kernel...
        Initializing cgroup subsys cpu
    Linux version 3.14.0-next-20140402+ (gcc version 4.8.3 20131111 (prerelease) (crosstool-NG linaro-1.13.1-4.8-2013.11 -
    Linaro GCC 2013.10) ) #1 SMP PREEMPT Fri Apr 11 15:39:25 BST 2014
    CPU: AArch64 Processor [410fd0f0] revision 0
    bootconsole [earlycon0] enabled
    efi: Getting parameters from FDT:
    efi:   System Table: 0x00000008ffffef18
    efi:   MemMap Address: 0x00000008fa366018
    efi:   MemMap Size: 0x00000930
    efi:   MemMap Desc. Size: 0x00000030
    efi:   MemMap Desc. Version: 0x00000001
    EFI v2.40 by ARM Fixed Virtual Platform EFI Apr 11 2014 11:55:10
    efi:
    Processing EFI memory map:
      0x000080000000-0x000080000fff [Loader Data]
      0x000080001000-0x00008007ffff [Conventional Memory]
      0x000080080000-0x000080655fff [Loader Data]
      0x000080656000-0x000087ffffff [Conventional Memory]
      0x00008c000000-0x00009fdfffff [Conventional Memory]
      0x00009fe00000-0x00009fe03fff [Loader Data]
      0x00009fe04000-0x0000ffab0fff [Conventional Memory]
      0x0000ffab1000-0x0000ffffffff [Boot Data]*
      0x000880000000-0x0008fa365fff [Conventional Memory]
      0x0008fa366000-0x0008fa366fff [Loader Data]
      0x0008fa367000-0x0008fa910fff [Loader Code]
      0x0008fa911000-0x0008fafb2fff [Boot Code]*
      0x0008fafb3000-0x0008fafbefff [Runtime Code]*
      0x0008fafbf000-0x0008fafd7fff [Runtime Data]*
      0x0008fafd8000-0x0008faff4fff [Runtime Code]*
      0x0008faff5000-0x0008fc63cfff [Conventional Memory]
      0x0008fc63d000-0x0008fc74dfff [Boot Data]*
      0x0008fc74e000-0x0008fc7c0fff [Conventional Memory]
      0x0008fc7c1000-0x0008fc8b8fff [Boot Data]*
      0x0008fc8b9000-0x0008fc8d5fff [Conventional Memory]
      0x0008fc8d6000-0x0008fc906fff [Boot Data]*
      0x0008fc907000-0x0008fc91efff [Conventional Memory]
      0x0008fc91f000-0x0008fc937fff [Boot Data]*
      0x0008fc938000-0x0008fc975fff [Conventional Memory]
      0x0008fc976000-0x0008fc981fff [Boot Data]*
      0x0008fc982000-0x0008fc98efff [Conventional Memory]
      0x0008fc98f000-0x0008fc991fff [Boot Data]*
      0x0008fc992000-0x0008fc9b0fff [Conventional Memory]
      0x0008fc9b1000-0x0008fce31fff [Boot Data]*
      0x0008fce32000-0x0008fce80fff [Conventional Memory]
      0x0008fce81000-0x0008ff682fff [Boot Data]*
      0x0008ff683000-0x0008ff685fff [Conventional Memory]
      0x0008ff686000-0x0008ff686fff [Boot Data]*
      0x0008ff687000-0x0008ff698fff [Conventional Memory]
      0x0008ff699000-0x0008ff699fff [Boot Data]*
      0x0008ff69a000-0x0008ff69dfff [Conventional Memory]
      0x0008ff69e000-0x0008ff69efff [Boot Data]*
      0x0008ff69f000-0x0008ff69ffff [Conventional Memory]
      0x0008ff6a0000-0x0008ff6c4fff [Boot Data]*
      0x0008ff6c5000-0x0008ff6cafff [Conventional Memory]
      0x0008ff6cb000-0x0008ffe28fff [Boot Data]*
      0x0008ffe29000-0x0008ffe8efff [Conventional Memory]
      0x0008ffe8f000-0x0008ffe91fff [Loader Data]
      0x0008ffe92000-0x0008fffb8fff [Boot Code]*
      0x0008fffb9000-0x0008fffccfff [Runtime Code]*
      0x0008fffcd000-0x0008ffffefff [Runtime Data]*
      0x0008fffff000-0x0008ffffffff [Boot Data]*
      0x00000c000000-0x00000ffeffff [Memory Mapped I/O]
      0x00001c170000-0x00001c170fff [Memory Mapped I/O]
    (...)

    Kernel command line: dtb=fvp-base-gicv2-psci.dtb console=ttyAMA0 earlyprintk=pl011,0x1c090000 debug uefi_debug
    root=/dev/vda2 rw
        (...)
    Memory: 3972540K/4128768K available (3912K kernel code, 269K rwdata, 1408K rodata, 199K init, 175K bss, 156228K
    reserved)
        (...)
        Remapping and enabling EFI services.
          EFI remap 0x0008fafb3000 => ffffffc87afb3000
          EFI remap 0x0008fafbf000 => ffffffc87afbf000
          EFI remap 0x0008fafd8000 => ffffffc87afd8000
          EFI remap 0x0008fffb9000 => ffffffc87ffb9000
          EFI remap 0x0008fffcd000 => ffffffc87ffcd000
          EFI remap 0x00000c000000 => ffffff8000080000
          EFI remap 0x00001c170000 => ffffff8000012000
          EFI freeing: 0x0000ffab1000-0x0000ffffffff
          EFI freeing: 0x0008fa911000-0x0008fafb2fff
          EFI freeing: 0x0008fc63d000-0x0008fc74dfff
          EFI freeing: 0x0008fc7c1000-0x0008fc8b8fff
          EFI freeing: 0x0008fc8d6000-0x0008fc906fff
          EFI freeing: 0x0008fc91f000-0x0008fc937fff
          EFI freeing: 0x0008fc976000-0x0008fc981fff
          EFI freeing: 0x0008fc98f000-0x0008fc991fff
          EFI freeing: 0x0008fc9b1000-0x0008fce31fff
          EFI freeing: 0x0008fce81000-0x0008ff682fff
          EFI freeing: 0x0008ff686000-0x0008ff686fff
          EFI freeing: 0x0008ff699000-0x0008ff699fff
          EFI freeing: 0x0008ff69e000-0x0008ff69efff
          EFI freeing: 0x0008ff6a0000-0x0008ff6c4fff
          EFI freeing: 0x0008ff6cb000-0x0008ffe28fff
          EFI freeing: 0x0008ffe92000-0x0008fffb8fff
          EFI freeing: 0x0008fffff000-0x0008ffffffff
        Freed 0x4384000 bytes of EFI boot services memory
        (...)
        Last login: Thu Feb 13 16:58:03 UTC 2014 on tty1
        root@genericarmv8:~# free -m
                    total       used       free     shared    buffers     cached
        Mem:          3963         80       3882          0          3         25
        -/+ buffers/cache:         52       3910
        Swap:            0          0          0
        root@genericarmv8:~#
    ```
