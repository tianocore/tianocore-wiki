# Change System Firmware Update Version

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)

One of the more common tasks is to update the system firmware version so the next time
a system firmware update capsule is built with new features or bug fixes, it has a new
version value, version string, and potentially prevents rolling back to earlier firmware
versions.  This version information is implemented in the `.aslc` file associated with the
System Firmware Descriptor PEIM.  The implementation of this PEIM is covered
[here](https:Capsule-Based-System-Firmware-Update-Implementation#system-firmware-descriptor-peim).

Change Version Value and Version String
=======================================

The following is a portion of the `SystemFirmwareDescriptor.aslc` that contains the version
values and strings.

```c
#define CURRENT_FIRMWARE_VERSION            0x00000010
#define CURRENT_FIRMWARE_VERSION_STRING     L"Version 0.0.0.16"
#define LOWEST_SUPPORTED_FIRMWARE_VERSION   0x0000000A
```

* **`CURRENT_FIRMWARE_VERSION`** - The hex value (e.g. `0x00000002`) of the firmware in
the system firmware update capsule.
* **`CURRENT_FIRMWARE_VERSION_STRING`** - A Unicode string that is the name of the firmware
version in system firmware update capsule.  This Unicode string is only used to display more
detailed version information.
* **`LOWEST_SUPPORTED_FIRMWARE_VERSION`** - A hex value(e.g. `0x00000002`) that is the lowest
`CURRENT_FIRMWARE_VERSION` value that this firmware version allows to be rolled back to.
For example, if the firmware on a platform has a `CURRENT_FIRMWARE_VERSION` of `0x00000010`
and a `LOWEST_SUPPORTED_FIRMWARE_VERSION` of `0x0000000A`, then the current firmware would
allow system firmware update capsule's with `CURRENT_FIRMWARE_VERSION` values of `0x000000A`
or higher to be installed.

Verify Version Value and Version String
=======================================

Build a firmware image with the updated version information and update a target platform with
the new version of firmware.  Run `CapsuleApp.efi -P` to view the Firmware Management Protocol
details. In the example below,

* **`CURRENT_FIRMWARE_VERSION`** is in the field called `Version` with a value of `0x10`.
* **`CURRENT_FIRMWARE_VERSION_STRING`** is in the field called `VersionName` with a value of `"Version 0.0.0.16"`.
* **`LOWEST_SUPPORTED_FIRMWARE_VERSION`** is in the field called `LowestSupportedImageVersion` with a value of `0xA`.

```text
### FMP DATA

FMP (0) ImageInfo:
  DescriptorVersion  - 0x3
  DescriptorCount    - 0x1
  DescriptorSize     - 0x60
  PackageVersion     - 0xFFFFFFFF
  PackageVersionName - "Verify Test Signing Key"
  ImageDescriptor (0)
    ImageIndex                  - 0x1
    ImageTypeId                 - 553B20F9-9154-46CE-8142-80E2AD96CD92
    ImageId                     - 0x4B545F4B52415551
    ImageIdName                 - "QuarkPlatformFdVerifyTestSigningKey"
    Version                     - 0x10
    VersionName                 - "Version 0.0.0.16"
    Size                        - 0x800000
    AttributesSupported         - 0xF
      IMAGE_UPDATABLE           - 0x1
      RESET_REQUIRED            - 0x2
      AUTHENTICATION_REQUIRED   - 0x4
      IN_USE                    - 0x8
      UEFI_IMAGE                - 0x0
    AttributesSetting           - 0xF
      IMAGE_UPDATABLE           - 0x1
      RESET_REQUIRED            - 0x2
      AUTHENTICATION_REQUIRED   - 0x4
      IN_USE                    - 0x8
      UEFI_IMAGE                - 0x0
    Compatibilities             - 0x0
      COMPATIB_CHECK_SUPPORTED  - 0x0
    LowestSupportedImageVersion - 0xA
    LastAttemptVersion          - 0x0
    LastAttemptStatus           - 0x0 (Success)
    HardwareInstance            - 0x0
FMP (0) PackageInfo - Unsupported
```

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)
