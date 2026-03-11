# Change Esrt System Firmware Update Guid

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)

* Run `CapsuleApp.efi -E` to see current ESRT GUID in the `FwType` field.
* Generate a new GUID value.
* Change `IMAGE_TYPE_ID_GUID` in `SystemFirmwareDescriptor.aslc` to new GUID Value.

```c
#define IMAGE_TYPE_ID_GUID    { 0xdd3b39b6, 0xd919, 0x46b5, { 0x89, 0x62, 0x4b, 0xb6, 0xd2, 0x27, 0x9a, 0xf7 } }
```

* Set PCD `gEfiMdeModulePkgTokenSpaceGuid.PcdSystemFmpCapsuleImageTypeIdGuid` in Platform DSC
to the same new GUID value used for `IMAGE_TYPE_ID_GUID`.

```ini
[PcdsDynamicExDefault.common.DEFAULT]
!if $(CAPSULE_ENABLE)
  gEfiMdeModulePkgTokenSpaceGuid.PcdSystemFmpCapsuleImageTypeIdGuid|{0xb6, 0x39, 0x3b, 0xdd, 0x19, 0xd9, 0xb5, 0x46, 0x89, 0x62, 0x4b, 0xb6, 0xd2, 0x27, 0x9a, 0xf7}
!endif
```

* Build firmware image
* Update target with new firmware image
* Run `CapsuleApp -E` to see current ESRT GUID in the `FwType` field.

```admonish warning
If an attempt is made to use `CapsuleApp.efi <CapsuleImage>` to update the firmware using
a capsule that uses the old ESRT GUID value, the update will fail. Capsules must also be updated
with the new GUID value.
```

* Change `[FmpPayload]` section `IMAGE_TYPE_ID` field in Platform FDF file to the same new GUID
value used for `IMAGE_TYPE_ID_GUID` in the `SystemFirmwareDescriptor.aslc` file.
* Build capsule image.
* `CapsuleApp.efi <CapsuleImage>` for newly generated capsules works.

Back to [Capsule Based System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)
