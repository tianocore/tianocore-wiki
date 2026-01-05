# How To Build OVMF

Building [OVMF](OVMF.md) is fairly easy once you have installed a few pre-requisites. Please note, however, that building OVMF is not required if you are only interested in [running OVMF](How-to-run-OVMF "wikilink"), since we have provided pre-built binaries of OVMF.

Build Pre-requisites
--------------------

- A edk2 build tree capable of building UEFI images
  - If you are new to edk2 building then these [getting started](Getting-Started-with-EDK-II) instructions may be helpful.
- An ASL compiler configured in your edk2 build tree
  - Either the IASL compiler or the Microsoft ASL compiler can be used
    - For Unix-like operating systems, IASL is the only option:
      - For newer Linux distributions, you should be able to install iasl via the distribution's package management system. (This [page](Using-EDK-II-with-Native-GCC) may help.)
      - Or, the [Unix-like systems getting started guide](Unix-like-systems) includes details for installing IASL as well.
    - For Windows, you can download pre-built version of IASL compiler from [http://www.acpica.org](http://www.acpica.org).
    - For Windows, you can also download a pre-built version of the Microsoft ASL compiler from [http://www.acpi.info](http://www.acpi.info).

Choosing which version of OVMF to build
---------------------------------------

First decide which version of OVMF you will build. You can choose to build the IA32 processor architecture and/or the X64 processor architecture. You should take into account the processor architectures which your toolchain is capable of building. Depending upon which you select, you should modify the ACTIVE\_PLATFORM and TARGET\_ARCH in Conf/target.txt.

| ACTIVE\_PLATFORM           | TARGET\_ARCH | PEI code | DXE/UEFI code |
|----------------------------|--------------|----------|---------------|
| OvmfPkg/OvmfPkgIa32.dsc    | IA32         | IA32     | IA32          |
| OvmfPkg/OvmfPkgIa32X64.dsc | IA32 X64     | IA32     | X64           |
| OvmfPkg/OvmfPkgX64.dsc     | X64          | X64      | X64           |

Example: <code>Conf/target.txt</code> values to build x64 UEFI image for OVMF using GCC5 compiler:

<pre>
ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc
TARGET_ARCH           = X64
TOOL_CHAIN_TAG        = GCC5
</pre>

Once you have modified Conf/target.txt, you can run the build command.

    bash$ build

If successful, you should now have a OVMF.fd file under the Build sub-directory. The exact directory under the Build directory will depend upon the toolchain, dsc and processor architecture.

You can use OVMF.Fd to [run OVMF](How-to-run-OVMF "wikilink").

See Also
--------

- [https://github.com/tianocore/edk2/blob/master/OvmfPkg/README](https://github.com/tianocore/edk2/blob/master/OvmfPkg/README)

