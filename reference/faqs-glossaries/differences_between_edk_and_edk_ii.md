# Differences Between Edk And Edk Ii

Note: The original EFI Development Kit
([EDK](../external-resources/edk2.md)) is no longer
supported. This document is maintained for historical purposes. Please
use [EDK II](../external-resources/edk_ii.md) for
current UEFI development.

### What are the differences between EDK and EDK II?

The main differences are in build architecture. The build description
files (.dsc .inf, etc) have been enhanced. The
[EDK II](../external-resources/edk_ii.md) build can
understand EDK build description files, so an EDK II build can include
EDK source code (with limitations). There are also build tool
differences: EDK II supports a larger number of operating systems and
tool chains.

EDK II has richer libraries (MDELIB, etc.), which makes the source code
structure very different. EDK II also uses a package concept, so that
the directory and file layout is different.

EDK II uses Platform Configuration Database
([PCD](pcd.md)) for
parameterization and binary patch support, and supports. newer UEFI/PI
specifications (see table below for more info).

EDK II provides compatibility with EDK-style source code using
EdkCompatiblityPkg ([ECP](../external-resources/ecp.md)).
However, ECP support is being deprecated in current platforms. This is
possible because ECP provides binary compatibility for EDK through
libraies and thunk code.

EDK II is also designed to work with Doxygen to generate design level
specifications.

In addition, since EDK II supports later versions of the UEFI and PI
Specifications, there are newer protocols that will be part of EDK II
that do not exist in EDK.

### Are there similarities between EDK and EDK II?

The Platform Initialization boot execution phases are similar. EDK and
EDK II have a similar boot (SEC-PEI-DXE-TSL) and runtime flow. Protocol
interfaces supported in both EDK and EDK II will be the same. EDK II can
include EDK source code though the use of
[ECP](../external-resources/ecp.md).

The following table is a side by side comparison.

|  | EDK | EDK II |
| --- | --- | --- |
| Development OS | WinXP | WinXP/7/8/8.1/10, Linux, OS/X |
| Compiler/Linker | VS2003, VS2005, WinDDK | VS2003, VS2005, VS2008, VS2010, VS2013, VS2015, WinDDK, Intel, GCC4, GCC5, LLVM/CLANG |
| Build | nmake | nmake, gmake |
| Build Tools | C | POSIX C, Python |
| Target Platforms (open source) | NT32, DUET, | See [EDK II Platforms](../../platforms-packages/platform-ports/edk_ii_platforms.md) |
| Distribution | ZIP Files – Entire Tree Packages with XML metadata | [Packages](../../platforms-packages/core-packages/edkii_packages.md) Standard module distribution method (source/binary) XML – Allows packages/modules to be imported/exported to many build systems EDK Compatibility Package – Provides reuse of EDK source modules |
| Standards (UEFI) | EFI 1.10, UEFI 2.0, UEFI 2.1, Intel Framework, PI 1.0 | Focus on UEFI 2.3+/PI 1.2+ Spec Updates. Includes Support for EFI 1.10, UEFI 2.0+, PI 1.0+ & Intel Framework. See [UDK2018](../../releases-history/archives/udk2018.md) & [EDK II](../external-resources/edk_ii.md) for info on specific releases. |
| Library | binary .Lib files | Library Classes/Library Instances Maximize reuse of source code Use library instances to optimize for size or speed |
| Configuration Method | **#define** | Platform Configuration Database Maximize reuse of source code Allows platform developer/integrator to modify settings without modifying module sources |

### How can you find out what has changed between releases?

The release notes will have a general summary of features and fixes for
EDK II changes. See
[EDK II](../external-resources/edk_ii.md) for the
latest stable release and release notes.

### In the past there were Libraries associated with PPI

EDK II does not use that model.
