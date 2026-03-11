# Capsule Based System Firmware Update Dsc Fdf

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)

Platform DSC `[Defines]` Section
=================================

Add the following `CAPSULE_ENABLE` define to the `[Defines]` section with a default
value of `FALSE`.  The capsule-based system firmware update feature can be enabled by
passing in `-D CAPSULE_ENABLE` flag on to the EDK II build command.

```
  #
  # Used to enable/disable capsule update features.  The default is FALSE for disabled.
  # Add -D CAPSULE_ENABLE to the build command line to enable capsule update features.
  # The build process generates a capsule update image along with the UEFI application
  # CapsuleApp.efi.  These 2 files must be transferred to storage media to in order for
  # a user to boot to UEFI Shell and use CapsuleApp.efi to submit the signed capsule.
  # Once the system is rebooted, the signed capsule is authenticated and the firmware is
  # update with the new system firmware version.
  #
  DEFINE CAPSULE_ENABLE = FALSE
```

Platform DSC `[LibraryClasses]` Sections
========================================

Make sure the following library mappings are in the `[LibraryClasses]` section.  The path to
the `PlatformFlashAccessLib` must be updated to match the path to the implementation provided
in the previous step.  These library mappings are required to support building the modules that
provide the Firmware Management Protocol for the system firmware, the UEFI application to test
capsule-based firmware update, along with the modules that authenticate a signed system firmware
update capsule

```
[LibraryClasses]
  OpensslLib|CryptoPkg/Library/OpensslLib/OpensslLib.inf
  IntrinsicLib|CryptoPkg/Library/IntrinsicLib/IntrinsicLib.inf
  BaseCryptLib|CryptoPkg/Library/BaseCryptLib/BaseCryptLib.inf
!if $(CAPSULE_ENABLE)
  CapsuleLib|MdeModulePkg/Library/DxeCapsuleLibFmp/DxeCapsuleLib.inf
!else
  CapsuleLib|MdeModulePkg/Library/DxeCapsuleLibNull/DxeCapsuleLibNull.inf
!endif
  EdkiiSystemCapsuleLib|SignedCapsulePkg/Library/EdkiiSystemCapsuleLib/EdkiiSystemCapsuleLib.inf
  FmpAuthenticationLib|MdeModulePkg/Library/FmpAuthenticationLibNull/FmpAuthenticationLibNull.inf
  IniParsingLib|SignedCapsulePkg/Library/IniParsingLib/IniParsingLib.inf
  PlatformFlashAccessLib|<Your Platform Package>/Feature/Capsule/Library/PlatformFlashAccessLib/PlatformFlashAccessLibDxe.inf
```

Make sure the following library mappings are in the `[LibraryClasses.common.DXE_RUNTIME_DRIVER]`

```
[LibraryClasses.common.DXE_RUNTIME_DRIVER]
!if $(CAPSULE_ENABLE)
  CapsuleLib|MdeModulePkg/Library/DxeCapsuleLibFmp/DxeRuntimeCapsuleLib.inf
!endif
```

Platform DSC `[Pcds]` Sections
==============================

* Add the PCD `PcdEdkiiSystemFirmwareImageDescriptor` to the `[PcdsDynamicExDefault]`
section with a value of `{0x0}` and a maximum size large enough to hold the
`EDKII_SYSTEM_FIRMWARE_IMAGE_DESCRIPTOR` structure and its associated Unicode strings
that are implemented in the `.aslc` file described
[here](../capsule_based_system_firmware_update_implementation.md#system-firmware-descriptor-peim).

* Add the PCD `PcdSystemFmpCapsuleImageTypeIdGuid` to the same `[PcdsDynamicExDefault]`
section.  This PCD is an array of one or more `IMAGE_TYPE_ID_GUID` values.  In the simplest
configuration, this PCD is set to the one `IMAGE_TYPE_ID_GUID` value from the the `.aslc`
file described [here](../capsule_based_system_firmware_update_implementation.md#system-firmware-descriptor-peim).
The PCD value is an array of 16-bytes.

* Add the PCD `PcdEdkiiSystemFirmwareFileGuid` to the same `[PcdsDynamicExDefault]`
section.  This PCD is set to a GUID value that is an array of 16-bytes.  The GUID
value may be set to the same GUID value in the `FileGuid` statement from the System Firmware
Update Configuration INI file described
[here](../capsule_based_system_firmware_update_implementation.md#system-firmware-update-configuration-ini-file).
In the simplest configuration, this PCD is set in the DSC file to the `FileGuid` value from the
INI file.

**NOTE:** The INI file syntax does support more complex capsule-base system firmware update
scenarios where the capsule may contain multiple FFS files with update content to support
multiple platforms or boards.  For this use case, a platform module must detect the current
platform or board and set the appropriate `PcdEdkiiSystemFirmwareFileGuid` value.

The example below uses a `PcdEdkiiSystemFirmwareImageDescriptor` size of 0x100 bytes, a
single GUID value for `PcdSystemFmpCapsuleImageTypeIdGuid`, and a GUID value for
`PcdEdkiiSystemFirmwareFileGuid` that matches `FileGuid` statement in the INI file.

```
[PcdsDynamicExDefault.common.DEFAULT]
!if $(CAPSULE_ENABLE)
  gEfiSignedCapsulePkgTokenSpaceGuid.PcdEdkiiSystemFirmwareImageDescriptor|{0x0}|VOID*|0x100
  gEfiMdeModulePkgTokenSpaceGuid.PcdSystemFmpCapsuleImageTypeIdGuid|{0xc0, 0x20, 0xaf, 0x62, 0x16, 0x70, 0x4a, 0x42, 0x9b, 0xf8, 0x9c, 0xcc, 0x86, 0x58, 0x40, 0x90}
  gEfiSignedCapsulePkgTokenSpaceGuid.PcdEdkiiSystemFirmwareFileGuid|{0x31, 0x1c, 0x96, 0x20, 0xdc, 0x66, 0xa5, 0x48, 0x89, 0x1c, 0x25, 0x43, 0x8b, 0xde, 0x14, 0x30}
!endif
```

Platform DSC `[Components]` Sections
====================================

Add the platform specific System Firmware Descriptor PEIM implemented in an earlier step to
the `[Components]` section with the rest of the PEIMs.  **NOTE:** The example below uses a
PEI CPU architecture of IA32.  Replace IA32 with PEI CPU Architecture for your platform.

```
[Components.IA32]
!if $(CAPSULE_ENABLE)
  # FMP image descriptor
  <Your Platform Package>/Feature/Capsule/SystemFirmwareDescriptor/SystemFirmwareDescriptor.inf
!endif
```

Update BdsDxe.inf `` section to use the PKCS7 based FMP Authentication Library
from the `SecurityPkg`.  If `CAPSULE_ENABLE` is `FALSE`, then use the Null FMP Authentication Library.
If `CAPSULE_ENABLE` is `TRUE`, then add the EsrtDxe module, the SystemFirmwareUpdateDxe module,
and the UEFI Application CapsuleApp to the `[Components]` section.  **NOTE:** The example below uses a
DXE CPU architecture of X64.  Replace X64 with DXE CPU Architecture for your platform.

```
[Components.X64]
  MdeModulePkg/Universal/BdsDxe/BdsDxe.inf {
    
!if $(CAPSULE_ENABLE)
      FmpAuthenticationLib|SecurityPkg/Library/FmpAuthenticationLibPkcs7/FmpAuthenticationLibPkcs7.inf
!else
      FmpAuthenticationLib|MdeModulePkg/Library/FmpAuthenticationLibNull/FmpAuthenticationLibNull.inf
!endif
  }

!if $(CAPSULE_ENABLE)
  MdeModulePkg/Universal/EsrtDxe/EsrtDxe.inf

  SignedCapsulePkg/Universal/SystemFirmwareUpdate/SystemFirmwareReportDxe.inf {
    
      FmpAuthenticationLib|SecurityPkg/Library/FmpAuthenticationLibPkcs7/FmpAuthenticationLibPkcs7.inf
  }
  SignedCapsulePkg/Universal/SystemFirmwareUpdate/SystemFirmwareUpdateDxe.inf {
    
      FmpAuthenticationLib|SecurityPkg/Library/FmpAuthenticationLibPkcs7/FmpAuthenticationLibPkcs7.inf
  }

  MdeModulePkg/Application/CapsuleApp/CapsuleApp.inf {
    
      PcdLib|MdePkg/Library/DxePcdLib/DxePcdLib.inf
  }
!endif
```

Platform FDF PEI `[FV]` Section
===============================

Add the SystemFirmwareDescriptor PEIM implemented in a earlier step to the FV that
contains other PEIMs so it is dispatched on every boot.

```
!if $(CAPSULE_ENABLE)
  # FMP image descriptor
INF RuleOverride = FMP_IMAGE_DESC <Your Platform Package>/Feature/Capsule/SystemFirmwareDescriptor/SystemFirmwareDescriptor.inf
!endif
```

Platform FDF DXE `[FV]` Section
===============================

Add the EsrtDxe, SystemFirmwareReportDxe, and the **test signing key** file to the
FV contains other DXE modules so they are dispatched on every boot.

```
!if $(CAPSULE_ENABLE)
INF  MdeModulePkg/Universal/EsrtDxe/EsrtDxe.inf
INF  SignedCapsulePkg/Universal/SystemFirmwareUpdate/SystemFirmwareReportDxe.inf

FILE FREEFORM = PCD(gEfiSignedCapsulePkgTokenSpaceGuid.PcdEdkiiPkcs7TestPublicKeyFileGuid) {
     SECTION RAW = BaseTools/Source/Python/Pkcs7Sign/TestRoot.cer
     SECTION UI = "Pkcs7TestRoot"
     }
!endif
```

Platform FDF `[FV]` Section
===========================

Add the following `[FV]` sections to the platform FDF file to build the FVs that
contains FFS files with the update payloads.  These FVs are added to an FMP payload
that is wrapped into a capsule that is signed using a PKCS7 certificate.

* The `FILE RAW` statement with the `# PcdEdkiiSystemFirmwareFileGuid` comment must
be updated with the same GUID value as the `FileGuid` statements from the System Firmware
Update Configuration INI file described
[here](../capsule_based_system_firmware_update_implementation.md#system-firmware-update-configuration-ini-file).
In the simplest configuration, there is only one `FileGuid` GUID value used in the INI
file.  If an INI file describes updates for multiple platforms or boards, then multiple
`FileGuid` GUID value may be used.  A `FILE RAW` statement for each `FileGuid` value must be
provided in the `[FV]` section.

```
!if $(CAPSULE_ENABLE)
[FV.CapsuleDispatchFv]
FvAlignment        = 16
ERASE_POLARITY     = 1
MEMORY_MAPPED      = TRUE
STICKY_WRITE       = TRUE
LOCK_CAP           = TRUE
LOCK_STATUS        = TRUE
WRITE_DISABLED_CAP = TRUE
WRITE_ENABLED_CAP  = TRUE
WRITE_STATUS       = TRUE
WRITE_LOCK_CAP     = TRUE
WRITE_LOCK_STATUS  = TRUE
READ_DISABLED_CAP  = TRUE
READ_ENABLED_CAP   = TRUE
READ_STATUS        = TRUE
READ_LOCK_CAP      = TRUE
READ_LOCK_STATUS   = TRUE

INF  SignedCapsulePkg/Universal/SystemFirmwareUpdate/SystemFirmwareUpdateDxe.inf

[FV.SystemFirmwareUpdateCargo]
FvAlignment        = 16
ERASE_POLARITY     = 1
MEMORY_MAPPED      = TRUE
STICKY_WRITE       = TRUE
LOCK_CAP           = TRUE
LOCK_STATUS        = TRUE
WRITE_DISABLED_CAP = TRUE
WRITE_ENABLED_CAP  = TRUE
WRITE_STATUS       = TRUE
WRITE_LOCK_CAP     = TRUE
WRITE_LOCK_STATUS  = TRUE
READ_DISABLED_CAP  = TRUE
READ_ENABLED_CAP   = TRUE
READ_STATUS        = TRUE
READ_LOCK_CAP      = TRUE
READ_LOCK_STATUS   = TRUE

FILE RAW = 26961C31-66DC-48A5-891C-25438BDE1430 { # PcdEdkiiSystemFirmwareFileGuid
    FD = <Your Platform FD Name>
  }

FILE RAW = ce57b167-b0e4-41e8-a897-5f4feb781d40 { # gEdkiiSystemFmpCapsuleDriverFvFileGuid
    FV = CapsuleDispatchFv
  }

FILE RAW = 812136D3-4D3A-433A-9418-29BB9BF78F6E { # gEdkiiSystemFmpCapsuleConfigFileGuid
    <Your Platform Package>/Feature/Capsule/SystemFirmwareUpdateConfig/SystemFirmwareUpdateConfig.ini
  }
!endif
```

Platform FDF `[FmpPayload]` Section
===================================

Add the following `[FmpPayload]` section to the platform FDF file to build the FMP payload
and that is wrapped into a capsule that is signed using a PKCS7 certificate.

* The `IMAGE_TYPE_ID` statement with the `# PcdSystemFmpCapsuleImageTypeIdGuid` comment must
be set to the `IMAGE_TYPE_ID_GUID` value from the the `.aslc` file described
[here](../capsule_based_system_firmware_update_implementation.md#system-firmware-descriptor-peim).

```
!if $(CAPSULE_ENABLE)
[FmpPayload.FmpPayloadSystemFirmwarePkcs7]
IMAGE_HEADER_INIT_VERSION = 0x02
IMAGE_TYPE_ID             = 62af20c0-7016-424a-9bf8-9ccc86584090 # PcdSystemFmpCapsuleImageTypeIdGuid
IMAGE_INDEX               = 0x1
HARDWARE_INSTANCE         = 0x0
MONOTONIC_COUNT           = 0x2
CERTIFICATE_GUID          = 4AAFD29D-68DF-49EE-8AA9-347D375665A7 # PKCS7

FV = SystemFirmwareUpdateCargo
!endif
```

Platform FDF `[Capsule]` Section
================================

Add the following `[Capsule]` section to the platform FDF file to wrap the FMP payload
into capsule that is signed using a PKCS7 certificate.

```
!if $(CAPSULE_ENABLE)
[Capsule.<YourPlatformName>FirmwareUpdateCapsuleFmpPkcs7]
CAPSULE_GUID                = 6dcbd5ed-e82d-4c44-bda1-7194199ad92a # gEfiFmpCapsuleGuid
CAPSULE_FLAGS               = PersistAcrossReset,InitiateReset
CAPSULE_HEADER_SIZE         = 0x20
CAPSULE_HEADER_INIT_VERSION = 0x1

FMP_PAYLOAD = FmpPayloadSystemFirmwarePkcs7
!endif
```

Platform FDF `[Rule]` Section
=============================

Add the following `[Rule]` required to add the platform specific SystemFirmwareDescriptor
PEIM to the PEI FV.

```
[Rule.Common.PEIM.FMP_IMAGE_DESC]
  FILE PEIM = $(NAMED_GUID) {
     RAW BIN                  |.acpi
     PEI_DEPEX PEI_DEPEX Optional        $(INF_OUTPUT)/$(MODULE_NAME).depex
     PE32      PE32    Align=4K          $(INF_OUTPUT)/$(MODULE_NAME).efi
     UI       STRING="$(MODULE_NAME)" Optional
     VERSION  STRING="$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
  }
```

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)
