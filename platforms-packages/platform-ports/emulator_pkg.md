# EmulatorPkg

## Status

The goal of this project is to provide a common OS emulation
environment, to replace
[Nt32Pkg](../../archives/platforms-packages/nt32_pkg.md) and
[UnixPkg](../../archives/platforms-packages/unix_pkg.md).

| UEFI architecture | Operating System | Status     |
|-------------------|------------------|------------|
| IA32              | Unix             | Functional |
| IA32              | Windows          | Functional |
| X64               | Unix             | Functional |
| X64               | Windows          | Functional |

## Source Repository

[1](https://github.com/tianocore/edk2/tree/master/EmulatorPkg)

## How to Build & Run

`$ EmulatorPkg/build.sh`
`$ EmulatorPkg/build.sh run`

The build architecture will match your host machine's architecture.

On X64 host machines, you can build + run IA32 mode as well:

`$ EmulatorPkg/build.sh -a IA32`
`$ EmulatorPkg/build.sh -a IA32 run`

### Ubuntu build dependencies

To install the extra build dependencies required to build EmulatorPkg,
run:

`$ sudo apt-get install libx11-dev libxext-dev`
