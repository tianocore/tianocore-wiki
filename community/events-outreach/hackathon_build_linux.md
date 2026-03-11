# Linux/GCC

## Distro Options

### Ubuntu 16.04 - Native

If you are running Ubuntu 16.04 then you are ready to build.

### Ubuntu 16.04 Prebuilt Container

We have already built an Ubuntu 16.04 container specifically for the workshops at OSFC 2018. Once installed, you will
need to add gcc-multilib to that image. The password is `password`:

```bash
su root
apt install gcc-multilib
```

### Ubuntu 16.04 using Docker (lightweight option)

If you are comfortable running your own docker build, follow these instructions to install Docker, based on your
distribution. You can then run ```docker pull``` to get ```ubuntu:16.04```.

* [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
* [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [OpenSUSE](https://en.opensuse.org/SDB:Docker)
* [Arch Linux](https://wiki.archlinux.org/index.php/Docker)
* [Install From Binary](https://docs.docker.com/install/linux/docker-ce/binaries/)

Once Docker is installed, follow the [post-install steps](https://docs.docker.com/install/linux/linux-postinstall/).

Before starting a Docker container, confirm that your user has been added to the `docker` group. This step is
**required**.

## Build Steps

Run the script below from an empty directory.  The script clones the EDK II
repository from GitHub and downloads and unzips the binary support files for the
MinnowBoard MAX.  It then sets up the environment for EDK II builds and builds
the MinnowBoard MAX firmware and generates UEFI Capsules that can be used to
update the MinnowBoard MAX firmware and three sample devices.

```bash
apt-get update && apt-get install -y build-essential git nasm wget m4 bison flex uuid-dev python unzip acpica-tools gcc-multilib

git clone --recurse-submodules -b edk2-stable201808_GCC_Fixes https://github.com/mdkinney/edk2.git

wget https://firmware.intel.com/sites/default/files/MinnowBoard_MAX-0.97-Binary.Objects.zip
unzip MinnowBoard_MAX-0.97-Binary.Objects.zip
mv MinnowBoard_MAX-0.97-Binary.Objects Vlv2Binaries

mkdir Conf

export WORKSPACE=$PWD
export PACKAGES_PATH=$WORKSPACE/edk2:$WORKSPACE/\Vlv2Binaries
export EDK_TOOLS_PATH=$WORKSPACE/edk2/BaseTools

cd $WORKSPACE/edk2

make -C BaseTools

. edksetup.sh BaseTools

cd Vlv2TbltDevicePkg
./Build_IFWI.sh MNW2 Debug
```

Once all the code is downloaded and installed, only the following commands are
required to setup the environment.  Run these from the same directory used to
install the source and binaries.

```base
export WORKSPACE=$PWD
export PACKAGES_PATH=$WORKSPACE/edk2:$WORKSPACE/\Vlv2Binaries
export EDK_TOOLS_PATH=$WORKSPACE/edk2/BaseTools

cd $WORKSPACE/edk2

make -C BaseTools

. edksetup.sh BaseTools

```

Once the environment is setup, the MinnowBoard MAX firmware and capsules can be
rebuilt using the following commands.

* Build Debug Image

```base
cd Vlv2TbltDevicePkg
./Build_IFWI.sh MNW2 Debug
```

* Build Release Image

```bash
cd Vlv2TbltDevicePkg
./Build_IFWI.sh MNW2 Release
```

The generated firmware image is the `MNW2MAX_X64_D_0084_01_GCC.bin` file in
`edk2\Vlv2TbltDevicePkg\Stitch`

The CapsuleApp and generated UEFI Capsules are in `Build\Vlv2TbltDevicePkg\Capsules`

Now you are ready to [flash the image, generate a capsule, and test it!](hackathon_flash_image_and_generate_capsule.md)
