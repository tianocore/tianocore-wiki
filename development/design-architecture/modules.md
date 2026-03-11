# Modules

The [EDK](../../reference/external-resources/edk2.md) Module Structure
Explained

The following three tables explain the structure of the EDK source code.

- Module Maintainers: a list of who is responsible for the module. This
  reponsibility includes ensuring proper management of defects,
  enhancements and code quality of changes to code that falls within
  this module.
- Directory to Module Table: this table explains the relationship
  between the CVS directories and the EDK Module List
- Definition to Module Table: this table explains the definition of each
  module

Important Links: EFI and Framework Open Source Defect Resolution Process
EDK Project Roles and Responsibilities Questions?

|                    |                  |
|--------------------|------------------|
| \| Module          | Maintainer/Owner |
| ATA                | Stanely Chen     |
| BDS                | Charlie Xia      |
| CirrusLogic        | Charile Xia      |
| Compression        | Mike Rothman     |
| Console            | Charile Xia      |
| CSM                | Nickel Shao      |
| DataHub            | Charile Xia      |
| Debug              | Sean Shang       |
| Disk               | Charile Xia      |
| DxeCore            | Mike Rothman     |
| Dxelpl             | Mike Rothman     |
| EBC                | Hua Fang         |
| EDK IPF Tip        | Jason Liu        |
| FAT                | Penny Gao        |
| Flash              | Stanely Chen     |
| Generic Definition | Jiewan Yao       |
| Library            | Hua Fang         |
| Logo               | Michael Krau     |
| Memory Test        | Charile Xia      |
| Network            | Michael Krau     |
| PCI                | Penny Gao        |
| PeiCore            | Rui Sun          |
| Runtime            | Mike Rothman     |
| SCSI               | Stanely Chen     |
| Security           | Mike Rothman     |
| Shell              | Hua Fang         |
| StatusCode         | Mike Rothman     |
| Timer              | Jean Wang        |
| Tools              | Liming Gao       |
| UI                 | Mike Rothman     |
| USB                | Penny Gao        |
| Variable           | Stanely Chen     |
| WinNT              | Jean Wang        |
| Other              | Michael Krau     |

**EDK Module Maintainer List**

|  |  |
|----|----|
| \Edk\Foundation\Core\Dxe | DxeCore |
| \Edk\Foundation\Core\Pei | PeiCore |
| \Edk\Foundation\Cpu | IA32CPU |
| \Edk\Foundation\Efi | Generic definition |
| \Edk\Foundation\Framework | Generic definition |
| \Edk\Foundation\Guid | Refer to Definitions to Module table |
| \Edk\Foundation\Include | Refer to Definitions to Module table |
| \Edk\Foundation\Library\CustomizedDecompress | Compression |
| \Edk\Foundation\Library\Dxe | Library |
| \Edk\Foundation\Library\EfiCommonLib | Library |
| \Edk\Foundation\Library\Pei | Library |
| \Edk\Foundation\Library\RuntimeDxe\EfiRuntimeLib | Runtime |
| \Edk\Foundation\Ppi | Refer to Definitions to Module table |
| \Edk\Foundation\Protocol | Refer to Definitions to Module table |
| \Edk\Other\Maintained\Application | Application |
| \Edk\Other\Maintained\Tools | Tools |
| \Edk\Other\Maintained\Universal\Disk\FileSystem\Fat | FAT |
| \Edk\Sample\Bus\Pci\AtapiPassThru | ATA |
| \Edk\Sample\Bus\Pci\CirrusLogic | CirrusLogic |
| \Edk\Sample\Bus\Pci\IdeBus | ATA |
| \Edk\Sample\Bus\Pci\PciBus | PCI |
| \Edk\Sample\Bus\Pci\Uhci | USB |
| \Edk\Sample\Bus\Pci\Undi | Network |
| \Edk\Sample\Bus\Scsi | SCSI |
| \Edk\Sample\Bus\Usb | USB |
| \Edk\Sample\Bus\WinNtThunk | WinNT |
| \Edk\Sample\Chipset\WinNtThunk | WinNT |
| \Edk\Sample\Cpu\DebugSupport\Dxe\ia32 | Debug |
| \Edk\Sample\Cpu\DebugSupport\Dxe\ipf | Debug |
| \Edk\Sample\Cpu\WinNtThunk | WinNT |
| \Edk\Sample\Include | WinNT |
| \Edk\Sample\Library\Dxe\WinNt | WinNT |
| \Edk\Sample\Platform\Generic\Dxe\ConPlatform | Console |
| \Edk\Sample\Platform\Generic\Dxe\GenericBds | BDS |
| \Edk\Sample\Platform\Generic\Dxe\PlatformBds | WinNT |
| \Edk\Sample\Platform\Generic\Logo | Logo |
| \Edk\Sample\Platform\Generic\MonoStatusCode | StatusCode |
| \Edk\Sample\Platform\Generic\Pei\Capsule | Capsule |
| \Edk\Sample\Platform\Generic\RuntimeDxe\FvbService | Flash |
| \Edk\Sample\Platform\Generic\RuntimeDxe\StatusCode | StatusCode |
| \Edk\Sample\Platform\IPF | EDK IPF Tip |
| \Edk\Sample\Platform\Nt32 | WinNT |
| \Edk\Sample\Tools | Tools |
| \Edk\Sample\Universal\Console | Console |
| \Edk\Sample\Universal\DataHub | DataHub |
| \Edk\Sample\Universal\Debugger | Debug |
| \Edk\Sample\Universal\Disk | Disk |
| \Edk\Sample\Universal\DxeIpl | DxeIpl |
| \Edk\Sample\Universal\Ebc | EBC |
| \Edk\Sample\Universal\FirmwareVolume | Flash |
| \Edk\Sample\Universal\GenericMemoryTest | MemoryTest |
| \Edk\Sample\Universal\MonotonicCounter | Flash |
| \Edk\Sample\Universal\Network | Network |
| \Edk\Sample\Universal\Runtime | Runtime |
| \Edk\Sample\Universal\Security | Security |
| \Edk\Sample\Universal\UserInterface | UI |
| \Edk\Sample\Universal\Variable | Variable |
| \Edk\Sample\Universal\WatchdogTimer | Timer |

**Directory-to-Module Table**

|  |  |
|----|----|
| Edk\Foundation\Ppi\BaseMemoryTest | MemoryTest |
| Edk\Foundation\Ppi\FlashMap | Flash |
| Edk\Foundation\Ppi\PeiInMemory | DxeIpl |
| Edk\Foundation\Ppi\Security | Security |
| Edk\Foundation\Ppi\StatusCodeMemory | StatusCode |
| Edk\Foundation\Guid\AcpiTableStorage | ACPI |
| Edk\Foundation\Guid\AlternateFvBlock | Flash |
| Edk\Foundation\Guid\Bmp | Logo |
| Edk\Foundation\Guid\BootState | BDS |
| Edk\Foundation\Guid\Capsule | Capsule |
| Edk\Foundation\Guid\CompatibleMemoryTested | BDS |
| Edk\Foundation\Guid\ConsoleInDevice | Console |
| Edk\Foundation\Guid\ConsoleOutDevice | Console |
| Edk\Foundation\Guid\DataHubRecords | DataHub |
| Edk\Foundation\Guid\EfiShell | Application |
| Edk\Foundation\Guid\FlashMapHob | Flash |
| Edk\Foundation\Guid\HotPlugDevice | Console |
| Edk\Foundation\Guid\IoBaseHob | IPF |
| Edk\Foundation\Guid\MemoryAllocationHob | ERM |
| Edk\Foundation\Guid\MemoryTypeInformation | DxeCore |
| Edk\Foundation\Guid\PciHotPlugDevice | PCI |
| Edk\Foundation\Guid\PciOptionRomTable | PCI |
| Edk\Foundation\Guid\PeiFlushInstructionCache | DxeCore |
| Edk\Foundation\Guid\PeiPeCoffLoader | DxeCore |
| Edk\Foundation\Guid\PeiPerformanceHob | Performance |
| Edk\Foundation\Guid\PeiTransferControl | DxeCore |
| Edk\Foundation\Guid\PrimaryConsoleInDevice | Console |
| Edk\Foundation\Guid\PrimaryConsoleOutDevice | Console |
| Edk\Foundation\Guid\PrimaryStandardErrorDevice | Console |
| Edk\Foundation\Guid\StandardErrorDevice | Console |
| Edk\Foundation\Guid\StatusCode | StatusCode |
| Edk\Foundation\Guid\StatusCodeCallerId | StatusCode |
| Edk\Foundation\Guid\SystemNvDataGuid | Flash |
| Edk\Foundation\Include\Ebc | Ebc |
| Edk\Foundation\Include\EfiCommon.h | Generic definition |
| Edk\Foundation\Include\EfiDebug.h | Debug |
| Edk\Foundation\Include\EfiDepex.h | Generic definition |
| Edk\Foundation\Include\EfiFlashMap.h | Flash |
| Edk\Foundation\Include\EfiPerf.h | Performance |
| Edk\Foundation\Include\EfiPxe.h | Network |
| Edk\Foundation\Include\EfiSpec.h | Generic definition |
| Edk\Foundation\Include\EfiStdArg.h | Generic definition |
| Edk\Foundation\Include\EfiVariable.h | Variable |
| Edk\Foundation\Include\EfiWorkingBlockHeader.h | Flash |
| Edk\Foundation\Include\Ia32 | Generic definition |
| Edk\Foundation\Include\IndustryStandard\Acpi.h | ACPI |
| Edk\Foundation\Include\IndustryStandard\pci22.h | PCI |
| Edk\Foundation\Include\IndustryStandard\scsi.h | SCSI |
| Edk\Foundation\Include\IndustryStandard\Tcpa11.h | Security |
| Edk\Foundation\Include\IndustryStandard\usb.h | USB |
| Edk\Foundation\Include\Ipf | Generic definition |
| Edk\Foundation\Include\Pei | Generic definition |
| Edk\Foundation\Include\Tiano.h | Generic definition |
| Edk\Foundation\Include\TianoApi.h | Generic definition |
| Edk\Foundation\Include\TianoCommon.h | Generic definition |
| Edk\Foundation\Include\TianoDevicePath.h | Generic definition |
| Edk\Foundation\Include\TianoError.h | Generic definition |
| Edk\Foundation\Include\TianoTypes.h | Generic definition |
| Edk\Foundation\Protocol\ConsoleControl | Console |
| Edk\Foundation\Protocol\CpuIO | Generic definition |
| Edk\Foundation\Protocol\CustomizedDecompress | Compression |
| Edk\Foundation\Protocol\DebugAssert | Debug |
| Edk\Foundation\Protocol\DebugMask | Debug |
| Edk\Foundation\Protocol\DiskInfo | Disk |
| Edk\Foundation\Protocol\EfiOEMBadging | Logo |
| Edk\Foundation\Protocol\ExtendedSalBootService | Runtime |
| Edk\Foundation\Protocol\ExtendedSalGuid | Runtime |
| Edk\Foundation\Protocol\FaultTolerantWriteLite | Flash |
| Edk\Foundation\Protocol\FirmwareVolumeDispatch | Flash |
| Edk\Foundation\Protocol\FvbExtension | Flash |
| Edk\Foundation\Protocol\GenericMemoryTest | MemoryTest |
| Edk\Foundation\Protocol\GuidedSectionExtraction | Flash |
| Edk\Foundation\Protocol\IdeControllerInit | ICH |
| Edk\Foundation\Protocol\IncompatiblePciDeviceSupport | PCI |
| Edk\Foundation\Protocol\IsaAcpi | IsaBus |
| Edk\Foundation\Protocol\IsaIo | IsaBus |
| Edk\Foundation\Protocol\LoadPe32Image | DxeCore |
| Edk\Foundation\Protocol\PciHostBridgeResourceAllocation | PCI |
| Edk\Foundation\Protocol\PciHotPlugInit | PCI |
| Edk\Foundation\Protocol\PciHotPlugRequest | PCI |
| Edk\Foundation\Protocol\PciPlatform | PCI |
| Edk\Foundation\Protocol\Performance | Performance |
| Edk\Foundation\Protocol\PlatformMemTest | MemoryTest |
| Edk\Foundation\Protocol\Print | UI |
| Edk\Foundation\Protocol\PxeDhcp4 | Network |
| Edk\Foundation\Protocol\PxeDhcp4Callback | Network |
| Edk\Foundation\Protocol\ScsiIo | SCSI |
| Edk\Foundation\Protocol\SecurityPolicy | Security |
| Edk\Foundation\Protocol\Tcp | Network |
| Edk\Foundation\Protocol\TianoDecompress | Compression |
| Edk\Foundation\Protocol\UgaSplash | BDS |
| Edk\Foundation\Protocol\UsbAtapi | USB |
| Edk\Foundation\Protocol\VariableStore | Variable |
| Edk\Foundation\Protocol\VirtualMemoryAccess | MemoryTest |

**Definition-to-Module Table**
