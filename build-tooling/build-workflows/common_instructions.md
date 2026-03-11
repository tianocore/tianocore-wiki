# Common Instructions

## Common EDK II Build Instructions for Linux

`Note: New build instructions are available. It is recommended to start with the new instructions if learning how to`
`build edk2 for the first time. This page is retained for reference.`

New instructions: [Build Instructions](build_instructions.md)

These instructions assume you have installed Linux packages required for
an [EDK II](../../reference/external-resources/edk_ii.md) build
environment, including git (example:
[Using EDK II with Native GCC](../environment-setup/using_edk_ii_with_native_gcc.md)).
The following instructions are common to the majority of Linux
environments.

### Get the edk2 source tree using Git

    bash$ mkdir ~/src
    bash$ cd ~/src
    bash$ git clone https://github.com/tianocore/edk2

Note: the 'git clone' command above pulls the latest code from edk2. If
you want to work from a stable release, specify a release tag when
cloning. Example:

    bash$ git clone https://github.com/tianocore/edk2.git vUDK2017

### Initialize submodules

    bash$ git submodule update --init

### Compile build tools

    bash$ cd ~/src/edk2
    bash$ make -C BaseTools
    bash$ . edksetup.sh

When the above steps are done, you can work in the edk2 directory for
code development.

### Build the EDK II BaseTools

    bash$ make -C edk2/BaseTools

### Setup build shell environment

    bash$ cd ~/src/edk2
    bash$ export EDK_TOOLS_PATH=$HOME/src/edk2/BaseTools
    bash$ . edksetup.sh BaseTools

### Modify Conf Files

Running `edksetup.sh` populates the `edk2/Conf` directory with default
configuration files. You will need to edit the `Conf/target.txt` file to
set the build platform, target architecture, tool chain, and
multi-threading options. The example below is based on building the
`MdeModulePkg` using GCC5.

#### Set Build Target Information

For the `Conf/target.txt` file, find the following lines:

    ACTIVE_PLATFORM       = Nt32Pkg/Nt32Pkg.dsc
    TOOL_CHAIN_TAG        = MYTOOLS

And change the corresponding lines to match these:

    ACTIVE_PLATFORM       = MdeModulePkg/MdeModulePkg.dsc
    TOOL_CHAIN_TAG        = GCC5

Note: The `gcc --version` command can be used to find out your GCC
version. Use the **GCC45** toolchain for gcc 4.5.\* and the **GCC46**
toolchain for gcc 4.6.\*.

Note: for GCC5 please install the gcc-5 package. Example for Ubuntu:
`sudo apt-get install gcc-5`

Locate the TARGET_ARCH setting:

    TARGET_ARCH           = IA32

Change this reflect the build architecture for the final UEFI binary.

Example: `X64`, `IA32 X64` (which will build both architectures).

Optional: enable multi-threaded build. The default value for
`MAX_CONCURRENT_THREAD_NUMBER` is 1, which disables multi-threaded
build. Change this value based on your system's multi-threading
capabilities. The formula is '1 + (2 x processor threads)'.

Example: for an Intel Core i5 (two processor cores w/ hyperthreading),
the value is `9`.

### Build Hello World! (and the rest of MdeModulePkg)

Now you should be able to simply run the build command to compile
`MdeModulePkg`.

    bash$ build

One result of the build is that you should have the HelloWorld UEFI
application:

    bash$ ls Build/MdeModule/DEBUG_*/*/HelloWorld.efi

### Build OVMF

Once your build environment is set up you might be interested in
[building the OVMF platform](../../development/tutorials-howto/how_to_build_ovmf.md) which
is included in the main
[EDK II](../../reference/external-resources/edk_ii.md) source tree.
Since [OVMF](../../platforms-packages/platform-ports/ovmf.md) builds a
full system firmware image, this may be of interest to UEFI system
firmware developers.
