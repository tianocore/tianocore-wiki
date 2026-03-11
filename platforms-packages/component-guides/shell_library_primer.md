# ShellPkg Main Libraries

There are 3 main [libraries](../../reference/external-resources/edk2_libraries.md) used by almost all shell commands and 2
of these are very usable by shell applications in the
[ShellPkg](../core-packages/shell_pkg.md).

## ShellLib

Used by both commands and applications. Provides easy access to Shell
Protocols, File I/O, and printing to the user. This library is
functional across both EDK Shell and UEFI Shell and is very useful tor
creating applications that are needed to function across Shell
environments.

## HandleParsingLib

Used by both commands and applications. Provides easy access for
manipulating handles. This means things like

- conversion the handle to and from the handle index; the index is the 2
  digit hex identifier of the handle seen by the user
- sorting of handles to determine parent controllers, child controllers,
  or which driver controls a given controller

## ShellCommandLib

fully usable only when linked within the shell binary and therefore only
by internal shell commands. This provides extra features for internal
commands. See
[Shell Application to
internal command](shell.md).

## ShellPkg Other Libraries

There are a couple of other libraries that are less commonly used.

## ShellCEntryPointLib

provides a C-looking entrypoint for an application. This should not be
used for porting full C applications.

## SortLib

provides quick sorting and comparison operations.

## PathLib

provides quick file path manipulation operations.

- remove ".", "..", and "/" from file paths.
- eliminate the last item in a path.

## ShellPkg Command Libraries

These are all libraries of shell commands and are only intended to be
linked directly into the shell. They are NULL-named libraries.

- UefiShellLevel1CommandsLib
- UefiShellLevel2CommandsLib
- UefiShellLevel3CommandsLib
- UefiShellDebug1CommandsLib
- UefiShellDriver1CommandsLib
- UefiShellInstall1CommandsLib
- UefiShellNetwork1CommandsLib
