# Tasks Emulatorpkg For Windows

Create a Windows based host environment for
[EmulatorPkg](../../platforms-packages/platform-ports/emulator_pkg.md).

- Status: Complete :heavy_check_mark:
- Difficulty: Medium
- Language: C
- Mentor:
- Suggested by: [https://github.com/ajfish](https://github.com/ajfish)

## Status

Completed :heavy_check_mark: by [https://github.com/niruiyu](https://github.com/niruiyu)
in 2018.

## Details

EmulatorPkg is very similar in many respects to the
[Nt32Pkg](../../archives/platforms-packages/nt32_pkg.md), but it
should be generic enough to replace both
[Nt32Pkg](../../archives/platforms-packages/nt32_pkg.md) and
[UnixPkg](../../archives/platforms-packages/unix_pkg.md).

This task would be to enable a Windows host environment for EmulatorPkg
so we can consider deprecating Nt32Pkg and UnixPkg in favor of using
EmulatorPkg.

## Development environment

Building: This project most likely would be completed on Windows with
Visual Studio C++.

It would also be desireable to be able to support building and running
the Windows host on Linux by building with mingw and running under Wine.
Therefore, it would also be possible to develop this project under
Linux.

Testing: Run the emulator on Windows or with Wine.

## Sub-goals

Some possible sub-goals for the driver

- Enter SEC phase of EmulatorPkg boot
- Enter PEI phase of EmulatorPkg boot
- Enter DXE phase of EmulatorPkg boot
- Enable multi-threading support
- Enable networking support

## Further Discussion

Please discuss the project on [https://edk2.groups.io/g/devel](https://edk2.groups.io/g/devel).

## See Also

- [Tasks](tasks.md)
- [GSoC2012#Port EmulatorPkg
  to Windows](../../community/events-outreach/gsoc2012.md#port-emulatorpkg-to-windows)
