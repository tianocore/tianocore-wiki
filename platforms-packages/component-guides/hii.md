# HII

Frequently asked questions about HII

### What is VFR?

Visual Forms Representation (VFR) it is a language like syntax for
describing forms and pages used for setup like screens. VFR is compiled
and produces Internal Forms Representation (IFR) The VFR Spec can be
downloaded from the [EDK-II-Specifications](../../reference/specs-standards/edk_ii_specifications.md) page

### What is VFR class & subclass?

Visual Forms Representation (VFR) Class and subclass described in a
FormSet definition.

- Valid Class names are: "NON_DEVICE" ,"DISK_DEVICE", "VIDEO_DEVICE",
  "NETWORK_DEVICE", "INPUT_DEVICE", "ONBOARD_DEVICE", "OTHER_DEVICE"

ii\.

- Valid SubClass names are: "SETUP_APPLICATION", "GENERAL_APPLICATION",
  "FRONT_PAGE", "SINGLE_USE"

### Are VFR generated manually or with tools?

Currently the VFR files are generated mostly manually with a text
editor.

### Can PEI check options set in NVRAM, that is, with PCDs?

Yes, the PCDs need to be declared as DynamicHii

### Where is an example of using HII and VFR ?

There is a good example in the MdkModulePkg\Universal\DriverSampleDxe.
To see the demo of this use the Nt32 emulation setup page, go to
"*Device Manager*" then "*Browser Testcase Engine*". This will open a
menu form with examples of different types of ways to enter
configuration data.
