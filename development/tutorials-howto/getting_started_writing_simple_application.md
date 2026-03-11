# Getting Started Writing Simple Application

## How to Write a Simple EDK II UEFI Application

### 1) Create a Work Space Directory

e.g. mkdir edk2

### 2) Download the Edk II source and build tools

1. Latest EDK II source from following Instructions on
    [Step by step instructions](getting_started_with_edk_ii.md)
    1. i.e. example \>git clone [https://github.com/tianocore/edk2.git](https://github.com/tianocore/edk2.git)

OR

1. Download the latest .zip
    [UDK2017](../../releases-history/archives/udk2017.md)
    Download release (or Latest
    [UDK](../../releases-history/archives/udk.md) release).

### 3) Run the **edksetup**

Run ***edksetup --nt32**'' script from the command line prompt at the Work Space directory

1. Windows Comand Prompt:
    **C:\edk2\> edksetup --nt32**
2. Linux like: **bash\$ .edksetup.sh BaseTools**

### 4) Edit the file conf/target.txt

Modify **TARGET_ARCH** and **TOOL_CHAIN_TAG** as required.

1. TOOL_CHAIN_TAG see:
    1. Windows
    [Windows systems ToolChain
    Matrix](../../build-tooling/environment-setup/windows_systems_toolchain_matrix.md)
    2. Newer versions of
    Linux[Using EDK II with Native GCC](../../build-tooling/environment-setup/using_edk_ii_with_native_gcc.md)
    3. Older Linux distributions
    [Unix-like systems](../../build-tooling/environment-setup/unix_like_systems.md)
    4. Mac OS X
        [Xcode](../../build-tooling/environment-setup/xcode.md)
2. TARGET_ARCH - Optional can also use
    **"-a"**
     on the BUILD command line
    1. Both IA32 and X64 :

        **TARGET_ARCH = IA32 X64**
    2. Just X64 :

        **TARGET_ARCH = X64**
    3. On the commnad line overriding the target.txt:
         **BUILD
        -a X64**

### 5) Create a project

1. Create a new directory. Can be a directory anywhere within the Work
    Space Directory (e.g. C:\edk2\\*MyHelloWorld* or
    ~/src/edk2/*MyHelloWorld*)
2. Create a .c file in the project directory (see example:
    [MyHelloWorld.c](getting_started_writing_myhelloworld_c.md))
3. Create a .inf file in the project directory (see examle:
    [MyHelloWorld.inf](getting_started_writing_myhelloworld_inf.md))

### 6) Build your UEFI Application

## Build X64 UEFI Application

1. Update an existing platform .DSC file with your project .inf file.
    The following list some examples.
    1. Edit the DuetPkg/DuetPkgX64.dsc and add your new application to
        the the  \[Components\]
         section and before the
         \[BuildOptions\]
        section. (e.g.
        MyHelloWorld/MyHelloWorld.inf )
2. Invoke the Build
    1. At the command prompt \>
        **Build -a X64 -p
        DuetPkg/DuetPkgX64.dsc**
3. Final Output .efi file will be in the directory
    ***WorkSpace*/Build/DuetPkg/DEBUG\_\$(*TOOL_CHAIN_TAG*)/X64**

## Build IA32 UEFI Application

1. Since this is the default as per the target.txt Update the
    Nt32Pkg/Nt32Pkg.dsc file.
    1. Edit the Nt32Pkg/Nt32Pkg.dsc and add your new application to the
        the  \[Components\]
        section and before the
        \[BuildOptions\]  section. (e.g.

        MyHelloWorld/MyHelloWorld.inf )
2. Invoke the Build
    1. At the command prompt \>
        **Build**
3. Final Output .efi file will be in the directory
    ***WorkSpace*/Build/NT32/DEBUG\_\$(*TOOL_CHAIN_TAG*)/IA32**
4. Test with Windows NT 32 emulation: command prompt \>
    **Build Run**

[Getting Started](getting_started.md)
