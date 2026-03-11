# Capsule Based System Firmware Update Implementation

Back to [Capsule Based System Firmware Update](../development/tutorials-howto/capsule_based_system_firmware_update.md)

Supporting [`SignedCapsulePkg`](https://github.com/tianocore/edk2/tree/master/SignedCapsulePkg) in an EDK II project
requires several platform-specific items.

## PlatformFlashAccessLib Library Instance

In order for a signed capsule to update the non-volatile storage device that that contains the system firmware contents
that may be updated, an instance of the `PlatformFlashAccessLib` must be implemented. This library class provides a
single API to update a portion of the non-volatile storage device.

```c
EFI_STATUS
EFIAPI
PerformFlashWrite (
  IN PLATFORM_FIRMWARE_TYPE       FirmwareType,
  IN EFI_PHYSICAL_ADDRESS         FlashAddress,
  IN FLASH_ADDRESS_TYPE           FlashAddressType,
  IN VOID                         *Buffer,
  IN UINTN                        Length
  );
```

A template of the `PlatformFlashAccessLib` is provided in `SignedCapsulePkg`
([`SignedCapsulePkg/Library/PlatformFlashAccessLibNull`](https://github.com/tianocore/edk2/tree/master/SignedCapsulePkg/Library/PlatformFlashAccessLibNull)).
Complete examples of this library can be found in the Intel Galileo Gen 2 & MinnowBoard Max EDK II projects:

  [`QuarkPlatformPkg/Feature/Capsule/Library/PlatformFlashAccessLib`](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/QuarkPlatformPkg/Feature/Capsule/Library/PlatformFlashAccessLib)
  [`Vlv2TbltDevicePkg/Feature/Capsule/Library/PlatformFlashAccessLib`](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/Vlv2TbltDevicePkg/Feature/Capsule/Library/PlatformFlashAccessLib)

It is recommended that this new platform specific library instance be placed in your platform package.

`<Your Platform Package>/Feature/Capsule/Library/PlatformFlashAccessLib`

## System Firmware Descriptor PEIM

A PEIM is used to provide a System Firmware Descriptor that provides the current system firmware version information.
This information is stored in a RAW section of the PEIM FFS file. The PEIM is responsible for reading the RAW section
and setting the `VOID*` PCD called `gEfiSignedCapsulePkgTokenSpaceGuid.PcdEdkiiSystemFirmwareImageDescriptor` to the
contents of the RAW section. The Firmware Management Protocol for the system firmware uses this PCD to determine the
current version of the system firmware.

Complete examples of this PEIM can be found in the Intel Galileo Gen 2 EDK II project:

  [`QuarkPlatformPkg/Feature/Capsule/SystemFirmwareDescriptor`](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/QuarkPlatformPkg/Feature/Capsule/SystemFirmwareDescriptor)

It is recommended that the System Firmware Descriptor PEIM implementation be placed under your platform package
directory.

`<Your Platform Package>/Feature/Capsule/SystemFirmwareDescriptor`

The System Firmware Descriptor information is implemented in the file `SystemFirmwareDescriptor.aslc`.
The version and string information must be updated for the system firmware update requirements.
The `IMAGE_TYPE_ID_GUID` define must be set to a new GUID value(using C structure syntax) for the
platform.  This GUID value must be one of the GUID values the platform supports for Capsule Base
System Firmware Updates.  The PCD called `gEfiMdeModulePkgTokenSpaceGuid.PcdSystemFmpCapsuleImageTypeIdGuid`
is set to an array of one or more of `IMAGE_TYPE_ID_GUID` values (using byte array syntax) in the
Platform DSC file described in the
[Platform DSC PCDs section](specs-standards/capsule_based_system_firmware_update_dsc_fdf.md#platform-dsc-pcds-sections).

The following `#defines` from `SystemFirmwareDescriptor.aslc` need to be updated for your platform:

```c
#define PACKAGE_VERSION                     0xFFFFFFFF
#define PACKAGE_VERSION_STRING              L"<Package Version String>"

#define CURRENT_FIRMWARE_VERSION            0x00000002
#define CURRENT_FIRMWARE_VERSION_STRING     L"<Current firmware version string>"
#define LOWEST_SUPPORTED_FIRMWARE_VERSION   0x00000001

#define IMAGE_ID                            SIGNATURE_64('_', '_', '_', '_', '_', '_', '_', '_')
#define IMAGE_ID_STRING                     L"<Image ID string>"

#define IMAGE_TYPE_ID_GUID    {0xe698490d, 0xc1fa, 0x47fd, { 0x86, 0x3, 0x69, 0xcd, 0x67, 0x8c, 0x5c, 0x39} }
```

## System Firmware Update Configuration INI File

This a System Firmware Update Configuration INI file provides the inventory of components in a capsule that are used by
the system firmware's Firmware Management Protocol to update one or more components in the system firmware's
non-volatile storage device using the `PlatformFlashAccessLib` instance implemented in the step above.

The INI file is ASCII text. The first section is `[Head]`. The value of `NumHeadUpdate` must be set to the number of
components in the inventory. There must be an `UpdateX` statement assigned to a unique name for each component in the
inventory. The example below shows a `[Head]` section that describes a single component.

```ini
[Head]
NumOfUpdate = 1
Update0 = MyPlatformFvMain
```

For each `UpdateX` statement, there must be section with the assigned name.  In this example
the unique component name is `MyPlatformFvMain`.  The component section provides the type
of firmware, address type, FFS file GUID from the capsule image to use, the address range of
FFS file to read, and the address range of the system's non-volatile storage device to update
using `PlatformFlashAccessLib` implemented above.  The example below shows the
`[MyPlatformFvMain]` section from the `[Head]` example above.

```ini
[MyPlatformFvMain]
FirmwareType = 0            # SystemFirmware
AddressType  = 0             # 0 - relative address, 1 - absolute address.
BaseAddress  = 0x00500000    # Base address offset on flash
Length       = 0x001E0000    # Length
ImageOffset  = 0x00500000    # Image offset of this SystemFirmware image
FileGuid     = 26961C31-66DC-48A5-891C-25438BDE1430  # PcdEdkiiSystemFirmwareFileGuid
```

* **`FirmwareType`**: `0` - SystemFirmware, `1` - NvRam.
* **`AddressType`**: `0` - relative address, `1` - absolute address.
* **`BaseAddress`**: Base address offset of non-volatile storage device in hexidecimal (e.g. `0x00400000`)
* **`Length`**: Image Length in hexidecimal (e.g. `0x00001000`)
* **`ImageOffset`**: Image offset of data in FileGuid FFS file in hexidecimal (e.g. `0x00400000`)
* **`FileGuid`**: Register format GUID of FFS File GUID in FmpPayload (e.g. `26961C31-66DC-48A5-891C-25438BDE1430`)

**NOTE:** A `[Name?]` section may have different `FileGuid`.  Only the ones whose `FileGuid` matches
`PcdEdkiiSystemFirmwareFileGuid` are used.  Non-matching ones are ignored.

Complete examples of System Firmware Update Configuration INI Files can be found the paths
listed below.  These examples build capsules that may be used for both system firmware
update and system firmware recovery.

* `QuarkPlatformPkg/Feature/Capsule/SystemFirmwareUpdateConfig/SystemFirmwareUpdateConfig.ini`
* `Vlv2TbltDevicePkg/Feature/Capsule/SystemFirmwareUpdateConfig/SystemFirmwareUpdateConfig.ini`
* `Vlv2TbltDevicePkg/Feature/Capsule/SystemFirmwareUpdateConfig/SystemFirmwareUpdateConfigGcc.ini`

It is recommended that the System Firmware Update Configuration INI file implementation be placed
in a platform package in the path `<Your Platform Package>/Feature/Capsule/SystemFirmwareUpdateConfig`.

Back to [Capsule Based System Firmware Update](../development/tutorials-howto/capsule_based_system_firmware_update.md)
