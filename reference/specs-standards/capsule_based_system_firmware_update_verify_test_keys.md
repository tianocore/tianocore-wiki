# Capsule Based System Firmware Update Verify Test Keys

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)

The following steps can be used to verify that the capsule-based system firmware update
feature has been integrated into a platform correctly. This example uses the [Intel® Galileo Gen 2](../../platforms-packages/platform-ports/galileo.md) platform.
These steps use the test signing keys, and it is a good idea to verify this update feature using the test signing keys
before using product specific signing keys.

```admonish important "Sequential Dependencies"
Each step in this sequence depends on all the previous steps. If any step in
this sequence does not match expectations, then debug and resolve the integration issue
before proceeding to the next step.
```

## OpenSSL Patch for `CryptoPkg`

This build process uses EDK II `CryptoPkg`, which requires a patch to be applied from OpenSSL. Please verify this
process has been completed before proceeding to the next step, otherwise the build will fail.

* EDK II CryptoPkg:
  [https://github.com/tianocore/edk2/tree/master/CryptoPkg](https://github.com/tianocore/edk2/tree/master/CryptoPkg)
* Patch Instructions:
  [https://github.com/tianocore/edk2/blob/master/CryptoPkg/Library/OpensslLib/Patch-HOWTO.txt](https://github.com/tianocore/edk2/blob/master/CryptoPkg/Library/OpensslLib/Patch-HOWTO.txt)

## Build and Boot Firmware Image

* Build firmware image setting the `-D CAPSULE_ENABLE` flag

`build -a IA32 -t VS2015x86 -p QuarkPlatformPkg/Quark.dsc -D CAPSULE_ENABLE`

* Update target with new firmware image
* Boot target to Boot Manager.  The front page should show a `WARNING: Test key detected.`
message informing the user that a test signing key is in use and that this firmware image
is only for development/debug purposes.  If logging is enabled, then this same message
is displayed in that log.

```
 QUARK

 Galileo 1.0.4                                       256 MB RAM

 WARNING: Test key detected.

   Select Language            <Standard English>         This is the option
                                                         one adjusts to change
   Device Manager                                        the language for the
   Boot Manager                                          current system
   Boot Maintenance Manager

   Continue
   Reset
```

* Boot target to UEFI Shell

## Verify Capsule Structures

* Copy `CapsuleApp.efi` to a USB drive
* Attach USB drive with`CapsuleApp.efi`
* Run `CapsuleApp.efi` with no parameters to see the help information

```
CapsuleApp:  usage
  CapsuleApp <Capsule...>
  CapsuleApp -S
  CapsuleApp -C
  CapsuleApp -P
  CapsuleApp -E
  CapsuleApp -G <BMP> -O <Capsule>
  CapsuleApp -N <Capsule> -O <NestedCapsule>
  CapsuleApp -D <Capsule>
Parameter:
  -S:  Dump capsule report variable (EFI_CAPSULE_REPORT_GUID),
       which is defined in UEFI specification.
  -C:  Clear capsule report variable (EFI_CAPSULE_RPORT_GUID),
       which is defined in UEFI specification.
  -P:  Dump UEFI FMP protocol info.
  -E:  Dump UEFI ESRT table info.
  -G:  Convert a BMP file to be a UX capsule,
       according to Windows Firmware Update document
  -N:  Append a Capsule Header to an existing capsule image,
       according to Windows Firmware Update document
  -O:  Output new Capsule file name
  -D:  Dump Capsule image header information and FMP header information,
       if it is an FMP capsule.
```

* Run `CapsuleApp.efi -P` to view the Firmware Management Protocol details.  The details
should match the System Firmware Descriptor PEIM .aslc file described
[here](../capsule_based_system_firmware_update_implementation.md#system-firmware-descriptor-peim).
In this example, the `ImageTypeId` GUID value is `553B20F9-9154-46CE-8142-80E2AD96CD92`, the
`Version` value is `0x3` and the `VersionName` string is `"0x00000003"`.

```
###### ######
## FMP DATA #
############
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
    Version                     - 0x3
    VersionName                 - "0x00000003"
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
    LowestSupportedImageVersion - 0x1
    LastAttemptVersion          - 0x0
    LastAttemptStatus           - 0x0 (Success)
    HardwareInstance            - 0x0
FMP (0) PackageInfo - Unsupported
```

* Run `CapsuleApp.efi -E` to view the ESRT details.  The `FwType` should be `0x1 (SystemFirmware)`,
and the `FwVersion` should be the `CURRENT_FIRMWARE_VERSION` value from the System Firmware
Descriptor PEIM .aslc file.  In this example the `FwClass` value is the same as the Firmware
Management Protocol `ImageTypeId` GUID value of `553B20F9-9154-46CE-8142-80E2AD96CD92`, and
the `FwVersion` value is `0x3`.

```
##############
## ESRT TABLE #
##############
EFI_SYSTEM_RESOURCE_TABLE:
FwResourceCount    - 0x1
FwResourceCountMax - 0x40
FwResourceVersion  - 0x1
EFI_SYSTEM_RESOURCE_ENTRY (0):
  FwClass                  - 553B20F9-9154-46CE-8142-80E2AD96CD92
  FwType                   - 0x1 (SystemFirmware)
  FwVersion                - 0x3
  LowestSupportedFwVersion - 0x1
  CapsuleFlags             - 0x1
    PERSIST_ACROSS_RESET   - 0x0
    POPULATE_SYSTEM_TABLE  - 0x0
    INITIATE_RESET         - 0x0
  LastAttemptVersion       - 0x0
  LastAttemptStatus        - 0x0 (Success)
```

## Build System Firmware Update Capsule

* Update System Firmware Descriptor PEIM .aslc file to a higher version by updating the
`CURRENT_FIRMWARE_VERSION` and `CURRENT_FIRMWARE_VERSION_STRING` defines.  This file is described
[here](../capsule_based_system_firmware_update_implementation.md#system-firmware-descriptor-peim)
* Build firmware image again setting the `-D CAPSULE_ENABLE` flag

`build -a IA32 -t VS2015x86 -p QuarkPlatformPkg/Quark.dsc -D CAPSULE_ENABLE`

## Verify System Firmware Update Capsule

* Copy System Firmware Update Capsule Image with higher version to a USB drive
* Run `CapsuleApp.efi -D <CapsuleImage>` to dump capsule image header information.  The
`UpdateImageTypeId` value is the same as the ESRT `FwClass` value is also the same as the
Firmware Management Protocol `ImageTypeId` GUID value of `553B20F9-9154-46CE-8142-80E2AD96CD92`.

```
[FmpCapusule]
CapsuleHeader:
  CapsuleGuid      - 6DCBD5ED-E82D-4C44-BDA1-7194199AD92A
  HeaderSize       - 0x20
  Flags            - 0x50000
  CapsuleImageSize - 0x84E535
FmpHeader:
  Version             - 0x1
  EmbeddedDriverCount - 0x0
  PayloadItemCount    - 0x1
  Offset[0]           - 0x10
FmpPayload[0] ImageHeader:
  Version                - 0x2
  UpdateImageTypeId      - 553B20F9-9154-46CE-8142-80E2AD96CD92
  UpdateImageIndex       - 0x1
  UpdateImageSize        - 0x84E4DD
  UpdateVendorCodeSize   - 0x0
  UpdateHardwareInstance - 0x0
```

* Run `CapsuleApp.efi <CapsuleImage>` to load and process the system firmware update capsule.

* If logging is enabled, then view the boot log to verify capsule processing.

* Run `CapsuleApp.efi -P` to view the Firmware Management Protocol details.  The details
should match the updated version information in the System Firmware Descriptor PEIM .aslc file.

```
############
## FMP DATA #
############
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
    Version                     - 0x4
    VersionName                 - "0x00000004"
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
    LowestSupportedImageVersion - 0x1
    LastAttemptVersion          - 0x0
    LastAttemptStatus           - 0x0 (Success)
    HardwareInstance            - 0x0
FMP (0) PackageInfo - Unsupported
```

* Run `CapsuleApp.efi -E` to view the ESRT details.  The details should match the updated
version information in the System Firmware Descriptor PEIM .aslc file.

```
##############
## ESRT TABLE #
##############
EFI_SYSTEM_RESOURCE_TABLE:
FwResourceCount    - 0x1
FwResourceCountMax - 0x40
FwResourceVersion  - 0x1
EFI_SYSTEM_RESOURCE_ENTRY (0):
  FwClass                  - 553B20F9-9154-46CE-8142-80E2AD96CD92
  FwType                   - 0x1 (SystemFirmware)
  FwVersion                - 0x4
  LowestSupportedFwVersion - 0x1
  CapsuleFlags             - 0x1
    PERSIST_ACROSS_RESET   - 0x0
    POPULATE_SYSTEM_TABLE  - 0x0
    INITIATE_RESET         - 0x0
  LastAttemptVersion       - 0x0
  LastAttemptStatus        - 0x0 (Success)
```

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)
