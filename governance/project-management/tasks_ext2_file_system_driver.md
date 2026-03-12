# Tasks Ext2 File System Driver

Develop an [https://en.wikipedia.org/wiki/Ext2](https://en.wikipedia.org/wiki/Ext2)
filesystem driver. (read-only support would be useful as a first step)

- Status: Complete :heavy_check_mark:
- Difficulty: Medium ... Hard
- Language: C
- Mentors: [https://github.com/mdkinney](https://github.com/mdkinney)
  and [https://github.com/corthon](https://github.com/corthon)
- Suggested by: rsun3

## Status

- Completed :heavy_check_mark:.
- Instead of implementing an ext2 driver as originally proposed, a much
  more useful [https://en.wikipedia.org/wiki/Ext4](https://en.wikipedia.org/wiki/Ext4)
  driver was implemented instead!
- Work done by Pedro Falcato ([https://github.com/heatd](https://github.com/heatd))
  as a [GSoC2021](../../community/events-outreach/gsoc2021.md)
  Student.

## Older Work

- Before GSoC 2021, some progress was made during
  [GSOC2011](../../community/events-outreach/gsoc2011.md) by
  Alin Rus
  - Source: [https://github.com/GunioRobot/Ext2Pkg](https://github.com/GunioRobot/Ext2Pkg)
- A newer effort has been started based on GRUB filesystem drivers,
  however this code cannot be merged to TianoCore due to GRUB being
  licensed under the GPL.
  - [https://github.com/pbatard/efifs/tree/master/](https://github.com/pbatard/efifs/tree/master/)

## Details

Filesystems in UEFI can be accessed through the SimpleFileSystem
protocol. This protocol is the defined within
[SimpleFileSystem.h](https://github.com/tianocore/edk2/blob/master/MdePkg/Include/Protocol/SimpleFileSystem.h).

This project should be completed with a BSD compatible solution. This
would mean that GPL licenses ext2 code or header files cannot be used.
You would need to investigate ext2's disk format by looking for
documentation, consulting with ext2(+) developers, or (as a last resort)
by examining the disk contents.

It may be possible to leverage BSD compatible code from a reputable open
source project, but using 3rd-party code does add some extra complexity
(see [Code Contributions](../../development/contribution-guides/code_contributions.md)).

## Development environment

Building: This project can be completed on any edk2 supported OS or
toolchain.

Testing:
[OVMF](../../platforms-packages/platform-ports/ovmf.md) would
probably provide the easiest environment for testing your project. You
should be able to build your driver into OVMF and then run OVMF with a
hard disk image that contains an ext2 file-system.

## Further Discussion

This project would be for an edk2 based driver, so please discuss the
project on [https://edk2.groups.io/g/devel](https://edk2.groups.io/g/devel).

## See Also

- [Tasks](tasks.md)
- [GSOC2011#Read-only ext2
  driver](../../community/events-outreach/gsoc2011.md)
