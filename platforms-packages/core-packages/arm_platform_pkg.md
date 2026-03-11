# ArmPlatformPkg

ArmPlatformPkg supports UEFI for ARM delveopment boards and their
drivers (ARM RealView EB/RTSM, ARM Versatile Express. This package also
exposes some common components (such as SEC, PEIMs, BDS) which can be
reused by ARM platforms to make the port easier. These components
support UniCore and MPCore and are tested on real hardware.

### How to port Tianocore/UEFI to a new ARM Platform

There are actually two cases:

- [Case 1 - EDK2 from cold boot](#arm_platform_porting_case1):
  the Tianocore project is used to produce the boot firmware that will
  control the boot process from the first instruction executed after
  cold boot to the start of the operating system on your platform
- [Case 2 - EDK2 as a 2nd
  stage Boot Loader](#arm_platform_porting_case2): the Tianocore project is only used to generate
  a 2nd (or 3rd) stage boot loader (such as the BeagleBoard)

![](ARM-EDK2-Overview.png)
*ARM-EDK2-Overview.png*

#### Case 1: EDK2 from cold boot

1\. Duplicate the ARM Platform reference DSC and FDF files

    cp ArmPlatformPkg/ArmPlatformPkg.dsc [YOUR_PLATFORM_PKG]/[YOUR_PLATFORM].dsc
    cp ArmPlatformPkg/ArmPlatformPkg.fdf [YOUR_PLATFORM_PKG]/[YOUR_PLATFORM].fdf

2\. Implement ArmPlatformLib by copying
ArmPlatformPkg/Library/ArmPlatformLibNull/

    cp -a ArmPlatformPkg/Library/ArmPlatformLibNull [YOUR_PLATFORM_PKG]/Library/[YOUR_PLATFORM]Lib

3\. Update ArmLib/ArmCpuLib/ArmPlatformLib to use the correct library in
your DSC file:

      ArmLib|ArmPkg/Library/ArmLib/ArmV7/ArmV7Lib.inf
      ArmCpuLib|ArmPkg/Drivers/ArmCpuLib/ArmCortexA9Lib/ArmCortexA9Lib.inf
      ArmPlatformLib|ArmPlatformPkg/Library/ArmPlatformLibNull/ArmPlatformLibNull.inf

- ArmLib can be :

`- ArmPkg/Library/ArmLib/ArmV7/ArmV7Lib.inf`
`- ArmPkg/Library/ArmLib/Arm11/Arm11Lib.inf`

- ArmCpuLib can be:

`- ArmPkg/Drivers/ArmCpuLib/Arm11MpCoreLib/Arm11MpCoreLib.inf`
`- ArmPkg/Drivers/ArmCpuLib/ArmCortexA8Lib/ArmCortexA8Lib.inf`
`- ArmPkg/Drivers/ArmCpuLib/ArmCortexA9Lib/ArmCortexA9Lib.inf`
`- ArmPkg/Drivers/ArmCpuLib/ArmCortexA15Lib/ArmCortexA15Lib.inf`

- ArmPlatformLib must be
  \[YOUR_PLATFORM_PKG\]/Library/\[YOUR_PLATFORM\]Lib/\[YOUR_PLATFORM\]Lib.inf

4\. Define the Secure / Monitor / Normal Stacks positions

      # Stacks for MPCores in Secure World
      gArmPlatformTokenSpaceGuid.PcdCPUCoresSecStackBase|0
      # Stacks for MPCores in Monitor Mode
      gArmPlatformTokenSpaceGuid.PcdCPUCoresSecMonStackBase|0
      # Stacks for MPCores in Normal World
      gArmPlatformTokenSpaceGuid.PcdCPUCoresStackBase|0

The Secure and Monitor stacks must be in Secure memory. And the Normal
Stack in your Scratch RAM (or any memory initialized at the early stage
of the boot process)

5\. Define the Base and the Size of your System Memory (RAM)

      gArmTokenSpaceGuid.PcdSystemMemoryBase|0
      gArmTokenSpaceGuid.PcdSystemMemorySize|0

6\. Implement and Update GicLib if required (if not ARM PL390 Generic
Interrupt Controller)

7\. Implement SerialPortLib, TimerLib, EfiResetSystemLib,
RealTimeClockLib for your platform

#### Case 2: EDK2 as a 2nd stage Boot Loader

1\. Duplicate the ARM Platform reference DSC and FDF files

    cp ArmPlatformPkg/ArmPlatformPkg-2ndstage.dsc [YOUR_PLATFORM_PKG]/[YOUR_PLATFORM].dsc
    cp ArmPlatformPkg/ArmPlatformPkg-2ndstage.fdf [YOUR_PLATFORM_PKG]/[YOUR_PLATFORM].fdf

2\. Implement ArmPlatformLib by copying
ArmPlatformPkg/Library/ArmPlatformLibNull/

    cp -a ArmPlatformPkg/Library/ArmPlatformLibNull [YOUR_PLATFORM_PKG]/Library/[YOUR_PLATFORM]Lib

3\. Update ArmLib/ArmCpuLib/ArmPlatformLib to use the correct library in
your DSC file:

      ArmLib|ArmPkg/Library/ArmLib/ArmV7/ArmV7Lib.inf
      ArmCpuLib|ArmPkg/Drivers/ArmCpuLib/ArmCortexA9Lib/ArmCortexA9Lib.inf
      ArmPlatformLib|ArmPlatformPkg/Library/ArmPlatformLibNull/ArmPlatformLibNull.inf

- ArmLib can be :

`- ArmPkg/Library/ArmLib/ArmV7/ArmV7Lib.inf`
`- ArmPkg/Library/ArmLib/Arm11/Arm11Lib.inf`

- ArmCpuLib can be:

`- ArmPkg/Drivers/ArmCpuLib/Arm11MpCoreLib/Arm11MpCoreLib.inf`
`- ArmPkg/Drivers/ArmCpuLib/ArmCortexA8Lib/ArmCortexA8Lib.inf`
`- ArmPkg/Drivers/ArmCpuLib/ArmCortexA9Lib/ArmCortexA9Lib.inf`
`- ArmPkg/Drivers/ArmCpuLib/ArmCortexA15Lib/ArmCortexA15Lib.inf`

- ArmPlatformLib must be
  \[YOUR_PLATFORM_PKG\]/Library/\[YOUR_PLATFORM\]Lib/\[YOUR_PLATFORM\]Lib.inf

4\. Define the Base and the Size of your System Memory (RAM)

      gArmTokenSpaceGuid.PcdSystemMemoryBase|0
      gArmTokenSpaceGuid.PcdSystemMemorySize|0

5\. Implement and Update GicLib if required (if not ARM PL390 Generic
Interrupt Controller)

6\. Implement SerialPortLib, TimerLib, EfiResetSystemLib,
RealTimeClockLib for your platform

### Source Repository

### Instruction to build ARM Versatile Express Development Board

[ArmPlatformPkg-ArmVExpressPkg](armplatformpkg_armvexpresspkg.md)
