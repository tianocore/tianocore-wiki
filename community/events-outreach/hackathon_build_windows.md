# Windows/Visual Studio

Run the script below from an empty directory.  The script clones the EDK II
repository from GitHub and downloads and unzips the binary support files for the
MinnowBoard MAX.  It then sets up the environment for EDK II builds and builds
the MinnowBoard MAX firmware and generates UEFI Capsules that can be used to
update the MinnowBoard MAX firmware and three sample devices.

```bash
git clone --recurse-submodules -b edk2-stable201808_GCC_Fixes https://github.com/mdkinney/edk2.git

powershell "& {[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; Invoke-WebRequest -Uri "https://indy.fulgan.com/SSL/openssl-1.0.2o-x64_86-win64.zip -OutFile openssl-1.0.2o-x64_86-win64.zip"}"
powershell Expand-Archive openssl-1.0.2o-x64_86-win64.zip

powershell "& {[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; Invoke-WebRequest -Uri "https://firmware.intel.com/sites/default/files/MinnowBoard_MAX-0.97-Binary.Objects.zip -OutFile MinnowBoard_MAX-0.97-Binary.Objects.zip"}"
powershell Expand-Archive MinnowBoard_MAX-0.97-Binary.Objects.zip .
sleep 1
powershell mv MinnowBoard_MAX-0.97-Binary.Objects Vlv2Binaries

powershell "& {[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; Invoke-WebRequest -Uri "https://www.nasm.us/pub/nasm/releasebuilds/2.13.03/win64/nasm-2.13.03-win64.zip -OutFile nasm-2.13.03-win64.zip"}"
powershell Expand-Archive nasm-2.13.03-win64.zip .

mkdir Conf

set PYTHON_HOME=c:\Python27

set WORKSPACE=%CD%
set EDK_TOOLS_PATH=%WORKSPACE%\edk2\BaseTools
set EDK_TOOLS_BIN=%EDK_TOOLS_PATH%\BinWrappers\WindowsLike
set PACKAGES_PATH=%WORKSPACE%\edk2;%WORKSPACE%\Vlv2Binaries
path=%path%;%EDK_TOOLS_PATH%\Bin\Win32;%WORKSPACE%\openssl-1.0.2o-x64_86-win64
set NASM_PREFIX=%WORKSPACE%\nasm-2.13.03\

cd %WORKSPACE%\edk2

call edkSetup.bat Rebuild

cd Vlv2TbltDevicePkg

Build_IFWI.bat /m /y MNW2 Debug
```

Once all the code and tools are downloaded and installed, only the following
commands are required to setup the environment.  Run these from the same
directory used to install the source and binaries.

```bash
set PYTHON_HOME=c:\Python27

set WORKSPACE=%CD%
set EDK_TOOLS_PATH=%WORKSPACE%\edk2\BaseTools
set EDK_TOOLS_BIN=%EDK_TOOLS_PATH%\BinWrappers\WindowsLike
set PACKAGES_PATH=%WORKSPACE%\edk2;%WORKSPACE%\Vlv2Binaries
path=%path%;%EDK_TOOLS_PATH%\Bin\Win32;%WORKSPACE%\openssl-1.0.2o-x64_86-win64
set NASM_PREFIX=%WORKSPACE%\nasm-2.13.03\

cd %WORKSPACE%\edk2

call edkSetup.bat Rebuild
```

Once the environment is setup, the MinnowBoard MAX firmware and capsules can be
rebuilt using the following commands.

* Build Debug Image

```bash
cd Vlv2TbltDevicePkg
Build_IFWI.bat /m /y MNW2 Debug
```

* Build Release Image

```bash
cd Vlv2TbltDevicePkg
Build_IFWI.bat /m /y MNW2 Release
```

The generated firmware image is the newest `.bin` file in `edk2/Vlv2TbltDevicePkg/Stitch`.
The file is in the form `MNW2MAX1.X64.0084.D01.<DATE>.bin`.

The CapsuleApp and generated UEFI Capsules are in `Build/Vlv2TbltDevicePkg/Capsules`

Now you are ready to [flash the image, generate a capsule, and test it!](hackathon_flash_image_and_generate_capsule.md)
