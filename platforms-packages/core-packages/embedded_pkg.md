# EmbeddedPkg

EmbeddedPkg provides protocol implementation for memory mapped
controllers and EBL. EBL (EDK Boot Loader) - a simpler EFI shell - is
aslo part of this package.

### EmbeddedPkg Protocols

#### EMBEDDEDEXTERNALDEVICE

> The only module today that implements this protocol is the OMAP3 PMIC.
> A reason to use this driver could be for instance you write a driver
> that depends on a standardized external device but not defined by the
> UEFI spec (I do not have one in mind) but you do not know where this
> external device is (case the device is memory mapped), this protocol
> allows to access this external device from your device driver in a
> platform independent way. But maybe this driver would need an
> additional parameter to identify the type of external device (the EFI
> Device Path will not help you to solve this limitation if the external
> device has not any EFI Device Path Node representation in the UEFI
> spec – probably all the External Device will have be VenHw(...) device
> path)..

Source: *edk2-devel mailing-list - Olivier Martin - Fri
03/08/2012 09:54*

#### EMBEDDEDDEVICEPROTOCOL

> One ‘limitation’ of most embedded system in comparison to PC system is
> most of their controllers are located at a different location in the
> memory map and they are no attached to enumerable bus (such as PCI).
> So it is quite difficult to know which device are present on a
> specific platform. This protocol is a tentative to get a solution to
> this limitation.

Source: *edk2-devel mailing-list - Olivier Martin - Fri
03/08/2012 09:54*

### Source Repository

### Related Pages

- EBL Support: [EmbeddedPkg-Ebl](embeddedpkg_ebl.md)
- FDT Support: [EmbeddedPkg-Fdt](embeddedpkg_fdt.md)
