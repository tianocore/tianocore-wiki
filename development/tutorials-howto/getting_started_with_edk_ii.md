# Getting Started With Edk Ii

```admonish note title="Updated Page"
New build instructions are available. It is recommended to start with the new instructions if learning how to
build edk2 for the first time. This page is retained for reference.
```

New instructions: [Build Instructions](../../build-tooling/build-workflows/build_instructions.md)

## Downloading and Compiling Code

This page shows the steps for downloading
[EDK II](../../reference/external-resources/edk_ii.md) from GitHub
and compiling projects under various OS/compiler environments.

## How to Setup a Local EDK II Tree

Several build environments are supported and documented. If instructions
are not available for your exact system configuration, you may still be
able to tweak the instructions to work on your system.

- Linux: [Using EDK II with Native GCC](../../build-tooling/environment-setup/using_edk_ii_with_native_gcc.md)
  (recommended for current versions of Linux)
- Microsoft Windows: [Windows systems](../../build-tooling/environment-setup/windows_systems.md) (Win7/8/8.1/10)
- Mac OS X: [Xcode](../../build-tooling/environment-setup/xcode.md)
- UNIX: [Unix-like systems](../../build-tooling/environment-setup/unix_like_systems.md) (For non-Linux UNIX,
  older Linux distros, or when using Cygwin)

**Note:** Some other build tools may be required depending on the
project or package:

- [Nasm](../../build-tooling/environment-setup/nasm_setup.md)
- [ASL
  Compiler](../../build-tooling/environment-setup/asl_setup.md)
- Install Python 3.7 or later ([https://www.python.org/](https://www.python.org/)) to run python
  tool from source
  - Python 2.7.10 or later can still be used with PYTHON_HOME

**Note:** Some of the examples use the
[Multiple_Workspace](../../build-tooling/build-workflows/multiple_workspace.md) \`PACKAGES_PATH\`
feature to the configure EDK II build environment. For example, this is
required for using platform code based on edk2-platforms:
([https://github.com/tianocore/edk2-platforms](https://github.com/tianocore/edk2-platforms)).

Once you have a basic build environment running, you can build a project
in RELEASE or
[DEBUG](edk_ii_debugging.md)
mode.

## GitHub Help

GitHub ([https://help.github.com/index.html](https://help.github.com/index.html)) provides step-by-step
instructions for user registration and basic features supported by
GitHub

- Setup GitHub for Linux/Windows/MAC
  ([https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git))
- Download and install a git GUI interface: git GUI Clients
  ([https://git-scm.com/download/gui/win](https://git-scm.com/download/gui/win)) \| TortoiseGit
  ([https://tortoisegit.org/](https://tortoisegit.org/))

### GitHub EDK II Project Repositories

- The EDK II project repository is available at
  [https://github.com/tianocore/edk2](https://github.com/tianocore/edk2).
- Prebuilt Windows tools are available at
  [https://github.com/tianocore/edk2-BaseTools-win32](https://github.com/tianocore/edk2-BaseTools-win32).
- [EDK
  II Platforms](../../platforms-packages/platform-ports/edk_ii_platforms.md) are available at
  [https://github.com/tianocore/edk2-platforms](https://github.com/tianocore/edk2-platforms).
- Content that is not released under an accepted open source license can
  be found at [https://github.com/tianocore/edk2-non-osi](https://github.com/tianocore/edk2-non-osi).

## EDK II Development Process

After setting up your build environment see
[EDK II Development Process](../contribution-guides/edk_ii_development_process.md) for
making contributions to the EDK II Project.

## Further Help

If you have questions about the code or run into obstacles getting
things to work, please join the EDK II developer
[Mailing-Lists](../../community/communications/mailing_lists.md) and ask your EDK II related
questions on the list.

For info on writing a simple UEFI EDK II Application, see:
[Getting Started
Writing Simple Application](getting_started_writing_simple_application.md)

To review the basic setup of .DSC, .DEC, and .INF files, see:
[Build Description Files](../../build-tooling/build-workflows/build_description_files.md)
