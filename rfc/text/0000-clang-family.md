# RFC: Add a CLANGPE Toolchain

## Metadata

- **RFC Number**: TBD
- **Title**: Add a CLANGPE Toolchain
- **Status**: Draft

## Change Log

- [2026-02-18]: Initial RFC created
- [2026-03-09]: Changed recommendation to CLANGPE instead of new CLANG FAMILY

## Motivation

Currently, `CLANGPDB` and `CLANGDWARF` are both in the `GCC` BaseTools FAMILY. This causes issues as many DSCs and INFs
reference `GCC:*_*_*_{CC|DLINK}_FLAGS` intending to either only apply overrides for `gcc` itself or for toolchains
building ELF binaries (`CLANGDWARF` and `GCC`). However, `CLANGPDB` is MSVC-like, not GNU-like and as a result will
fail to build with many flags passed in this fashion. `CLANGDWARF` is GNU-like and will generally pass, though there
are exceptions (such as the MinGW case) and in general this behavior is dangerous to hide one toolchain behind another
one as it is non-obvious and unknown behavior could result.

`CLANGPDB` should live in the `MSFT` FAMILY. However, it uses GNU-like input arguments for `CC_FLAGS` so it will break
when MSVC-like arguments are passed. `clang` can be configured to use `clang-cl` instead of `clang` directly and
`clang-cl` accepts MSVC-like arguments. `CLANGPDB` has an established user base and as such can not take a breaking
change to move to `clang-cl` and `MSFT` FAMILY.

Therefore, a new toolchain, `CLANGPE`, should be created that is a clone of `CLANGPDB` except it uses `clang-cl` and
is in the `MSFT` FAMILY. `CLANGPDB` should then be deprecated and eventually supported dropped for it

## Technology Background

BaseTools uses toolchain families as a way of referencing many different toolchains in one concise statement. For
example, VS2015 - VS2026 can be referenced with `MSFT:*_*_*_CC_FLAGS` instead of one line per toolchain. When
`CLANGPDB` and `CLANGDWARF` were added, they were both added to the `GCC` FAMILY.

The situation was aggravated when the `gcc` toolchains converged on `GCC` as the name of the specific toolchain,
replacing `GCC5`, because this led to the FAMILY and the toolchain name being the same, which adds complexity to
filtering for `gcc` only. For example, to specify a file should only be compiled for `CLANGPDB`, the following can
be added to an INF:

```inf
[Sources]
  MyFile.c | CLANGPDB
```

However, for `GCC`, this must be:

```inf
[Sources]
  MyFile.c | GCC | GCC
```

If only a single `GCC` is used, then `CLANGPDB` and `CLANGDWARF` will also build this file because the FAMILY is
matched first.

## Goals

List the specific goals this RFC aims to achieve. What defines success for this proposal?

1. Be able to clearly understand what toolchain is referenced in a DSC/INF statement
2. Prevent unintentional toolchain configuration from being applied to the wrong toolchain

## Requirements

1. Toolchain configuration for one toolchain should not be accidentally applied to another
2. `clang` should be treated as a first class toolchain in edk2

## UEFI/PI Specification Impact

This does not have UEFI/PI impact.

## Backward Compatibility

Creating a new toolchain will require going over existing DSCs and INFs and changing `[BuildOptions]` statements to
ensure the `MSFT` FAMILY is scoped correctly and that any `GCC` FAMILY statements that were intending to be applied to
`CLANGPDB` are accurately captured for `CLANGPDB`. While this is a breaking change, it also is a good exercise because
it will almost certainly catch cases where `GCC` toolchain configuration was applied incorrectly to `CLANGPDB`.

## Platform/Package Impact

This will affect all packages; the only code change is in BaseTools.

## Unresolved Questions

N/A.

## Prior Art/Related Work

All prior work has shoehorned `CLANGPDB` into the `GCC` FAMILY and figured out how to make that work.

## Alternatives

Do not create `CLANGPE` and continue with `CLANGPDB` only. This is a workable solution, but has the drawbacks listed
in the [motivation](#motivation) section.

## Implementation Design

The main implementation change is adding a new `CLANGPE` toolchain in BaseTools.

### Architecture Overview

`CLANGPE` is created as part of the `MSFT` FAMILY. `CLANGPDB` is deprecated.

### Detailed Design

As noted above, many DSCs and INFs need to be updated to include the new toolchain, if existing FAMILY statements
are not sufficient. BaseTools will have to be updated to understand the new toolchain. This is primarily in the
configuration files.

### Code Examples

The main changes will take statements like this from `UnitTestFrameworkPkgHost.dsc.inc` (following the eventual
deprecation of `CLANGPDB`):

```inf
  #
  # Set MSFT HOST_APPLICATION options
  #
  MSFT:*_*_*_CC_FLAGS     = $(VISUAL_STUDIO_DEFINES)
  #
  # Must override DLINK_FLAGS to remove /DLL when linking .exe
  #
  MSFT:*_*_*_DLINK_FLAGS   == /out:"$(BIN_DIR)\$(MODULE_NAME_GUID).exe" /NOLOGO /SUBSYSTEM:CONSOLE /WHOLEARCHIVE /STACK:0x00800000,0x00800000 /SAFESEH:NO
  MSFT:*_*_IA32_DLINK_FLAGS = $(VISUAL_STUDIO_IA32_LIB_PATHS)
  MSFT:*_*_X64_DLINK_FLAGS  = $(VISUAL_STUDIO_X64_LIB_PATHS)
  MSFT:*_*_*_DLINK_FLAGS    = /DEBUG /pdb:"$(BIN_DIR)\$(MODULE_NAME_GUID).pdb"

  #
  # Set CLANGPDB HOST_APPLICATION options
  #
  CLANGPDB:*_*_*_CC_FLAGS     = $(VISUAL_STUDIO_DEFINES)
  #
  # Must override DLINK_FLAGS to remove /DLL when linking .exe
  #
  CLANGPDB:*_*_*_DLINK_FLAGS   == /out:"$(BIN_DIR)\$(MODULE_NAME_GUID).exe" /NOLOGO /SUBSYSTEM:CONSOLE /WHOLEARCHIVE /STACK:0x00800000,0x00800000 /IGNORE:4099 /SAFESEH:NO
  CLANGPDB:*_*_IA32_DLINK_FLAGS = $(VISUAL_STUDIO_IA32_LIB_PATHS)
  CLANGPDB:*_*_X64_DLINK_FLAGS  = $(VISUAL_STUDIO_X64_LIB_PATHS)
  CLANGPDB:*_*_*_DLINK_FLAGS    = /DEBUG /pdb:"$(BIN_DIR)\$(MODULE_NAME_GUID).pdb"
```

and change them to:

```inf
#
  # Set MSFT HOST_APPLICATION options
  #
  MSFT:*_*_*_CC_FLAGS     = $(VISUAL_STUDIO_DEFINES)
  #
  # Must override DLINK_FLAGS to remove /DLL when linking .exe
  #
  MSFT:*_*_*_DLINK_FLAGS   == /out:"$(BIN_DIR)\$(MODULE_NAME_GUID).exe" /NOLOGO /SUBSYSTEM:CONSOLE /WHOLEARCHIVE /STACK:0x00800000,0x00800000 /SAFESEH:NO
  MSFT:*_*_IA32_DLINK_FLAGS = $(VISUAL_STUDIO_IA32_LIB_PATHS)
  MSFT:*_*_X64_DLINK_FLAGS  = $(VISUAL_STUDIO_X64_LIB_PATHS)
  MSFT:*_*_*_DLINK_FLAGS    = /DEBUG /pdb:"$(BIN_DIR)\$(MODULE_NAME_GUID).pdb"
```

Other changes will help define the difference between the `GCC` FAMILY and the `MSFT` FAMILY, like this in
`ArmVirtPkg.dsc.inc`:

```inf
[BuildOptions.common.EDKII.DXE_CORE,BuildOptions.common.EDKII.DXE_DRIVER,BuildOptions.common.EDKII.UEFI_DRIVER,BuildOptions.common.EDKII.UEFI_APPLICATION]
  GCC:*_GCC_*_DLINK_FLAGS = -z common-page-size=0x1000
  GCC:*_CLANGDWARF_*_DLINK_FLAGS = -z common-page-size=0x1000
  CLANGPDB:*_*_*_DLINK_FLAGS = /ALIGN:0x1000
```

becomes:

```inf
[BuildOptions.common.EDKII.DXE_CORE,BuildOptions.common.EDKII.DXE_DRIVER,BuildOptions.common.EDKII.UEFI_DRIVER,BuildOptions.common.EDKII.UEFI_APPLICATION]
  GCC:*_*_*_DLINK_FLAGS = -z common-page-size=0x1000
  MSFT:*_*_*_DLINK_FLAGS = /ALIGN:0x1000
```

## Testing Strategy

This is primarily a build feature; build breaks are expected as part of the development process. Testing will be
ensuring the virtual platforms in edk2 build and run successfully in their various flavors with all toolchains.

## Migration/Adoption Plan

This will be done all in one shot in edk2. Downstream platforms would need to change any platform DSCs and INFs when
adopting `CLANGPE`, if the `MSFT` FAMILY does not not already fully describe the scenarios required for `CLANGPE`. This
is not a breaking change for platforms and a signficant runway will be given to allow platforms to transition from
`CLANGPDB` to `CLANGPE` before `CLANGPDB` support is removed. 

## Guide-Level Explanation

The guidance for developers is:

When building with `CLANGPE`, ensure the `MSFT` FAMILY statements contain all the required configuration for `CLANGPE`.
