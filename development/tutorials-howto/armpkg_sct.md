# ArmPkg Sct

## Debugging UEFI Self Certification Test (SCT)

1\. Build UEFI for your targeted ARM RTSM (RealTime System Model). See
[ArmPlatformPkg-ArmVExpressPkg](../../platforms-packages/core-packages/armplatformpkg_armvexpresspkg.md)
for more complete instructions.

For this tutorial we will use ARM Versatile Express Cortex A9x4 RTSM:

    cd $EDK2_ROOT
    . edksetup.sh `pwd`/BaseTools
    build -a ARM -p ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.dsc -t ARMLINUXGCC

2\. Build SCT binaries. Rebuilding the binaries will ensure the SCT
package matches your SCT sources code that will ease the debugging. See
the README attached to your SCT source package for the correct build
instructions.

Example with the latest SCT source package (UEFI 2.3.1 SCT October
2012):

    export PATH=/opt/DS-5/bin:$PATH
    SctPkg/BuildArmSct.sh RVCT

**Note:** The first line is to add the ARM compiler (RVCT toolchain)
to your PATH. The location of your RVCT toolchain might be different.

3\. Create a MMC card with your newly built version of SCT and the EDK
Shell binary (required to start SCT)

    mkfs.vfat -C -n MMC_SD_CARD /tmp/mmc.dat 131072
    mkdir /tmp/fs
    sudo mount -o rw,loop=/dev/loop0,uid=`whoami`,gid=`whoami` /tmp/mmc.dat /tmp/fs

    cp $EDK2_ROOT/EdkShellBinPkg/FullShell/Arm/Shell_Full.efi /tmp/fs/
    cp -Rf $EDK2_ROOT/Build/UefiSct/DEBUG_RVCTLINUX/SctPackageARM/* /tmp/fs/

    sudo umount /tmp/fs

4\. Start the RTSM model with the MMC card that contains the SCT
binaries.

Example to start ARM Versatile Express Cortex A9x4 RTSM:

RTSM_VE_Cortex-A9_MPx4 -C
motherboard.flashloader0.fname=\$EDK2_ROOT/Build/ArmVExpress-RTSM-A9x4/DEBUG_ARMLINUXGCC/FV/RTSM_VE_CORTEX-A9_EFI.fd
-C motherboard.mmc.p_mmc_file="/tmp/mmc.dat" -C
motherboard.flashloader1.fname="flash.dat" -C
motherboard.flashloader1.fnameWrite="flash.dat"

**Note:** The arguments '-C
motherboard.flashloader1.fname="flash.dat" -C
motherboard.flashloader1.fnameWrite="flash.dat"' allow to save the
non-volatile UEFI environment variables (used by the UEFI Boot Manager)
within a file of your host machine.

5\. To install SCT, you must start EDK Shell. EDK Shell can be started
by EFI Shell, but we will choose to add the EDK Shell as the first boot
entry to allow SCT when restarted to resume its test execution.
Select the "Boot Manager" entry and "Add Boot Device Entry" to create a
new entry for the EDK Shell

    ```text
    [1] SemiHosting
            - VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)/zImage
            - Arguments:
            - LoaderType: 1
    [2] Shell
    [3] Boot Manager
    Start: Invalid input (max 3)
    Start: 3
    [1] Add Boot Device Entry
    [2] Update Boot Device Entry
    [3] Remove Boot Device Entry
    [4] Update FDT path
    [5] Return to main menu
    Choice: 1
    [1] SemihostFs (0 MB)
            - VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)
    [2] MMC_SD_CARD (127 MB)
            - VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)
    [3] VenHw(E7223039-5836-41E1-B542-D7EC736C5E59)
            - VenHw(E7223039-5836-41E1-B542-D7EC736C5E59)
    [4] VenHw(02118005-9DA7-443A-92D5-781F022AEDBB)
            - VenHw(02118005-9DA7-443A-92D5-781F022AEDBB)
    [5] VenHw(1F15DA3C-37FF-4070-B471-BB4AF12A724A)
            - VenHw(1F15DA3C-37FF-4070-B471-BB4AF12A724A)
    [6] VenHw(CC2CBF29-1498-4CDD-8171-F8B6B41D0909)
            - VenHw(CC2CBF29-1498-4CDD-8171-F8B6B41D0909)
    [7] VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)
            - VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)
    Select the Boot Device: 2
    File path of the EFI Application or the kernel: Shell_Full.efi
    Is your application is an OS loader? [y/n] n
    Description for this new Entry: EDK Shell
    ```

Delete the first entry to make the EDK Shell the first one:

    ```text
    [1] Add Boot Device Entry
    [2] Update Boot Device Entry
    [3] Remove Boot Device Entry
    [4] Update FDT path
    [5] Return to main menu
    Choice: 3
    [1] SemiHosting
            - VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)/zImage
            - Arguments:
    [2] EDK Shell
            - VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)/Shell_Full.efi
    Delete entry: 1
    [1] Add Boot Device Entry
    [2] Update Boot Device Entry
    [3] Remove Boot Device Entry
    [4] Update FDT path
    [5] Return to main menu
    Choice: 5
    [1] EDK Shell
            - VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)/Shell_Full.efi
            - LoaderType: 0
    [2] Shell
    [3] Boot Manager
    Start:
    ```

6\. Start EDK Shell to install SCT

    ```text
    EFI Shell version 2.31 [0.0]
    Current running mode 1.1.2
    Device mapping table
      fs0   :BlockDevice - Alias f5 blk0
             VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)
      fsnt0 :BlockDevice - Alias f8
             VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)
      blk0  :BlockDevice - Alias f5 fs0
             VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)
      blk1  :BlockDevice - Alias (null)
             VenHw(02118005-9DA7-443A-92D5-781F022AEDBB)
      blk2  :BlockDevice - Alias (null)
             VenHw(CC2CBF29-1498-4CDD-8171-F8B6B41D0909)
      blk3  :BlockDevice - Alias (null)
             VenHw(E7223039-5836-41E1-B542-D7EC736C5E59)
      blk4  :BlockDevice - Alias (null)
             VenHw(1F15DA3C-37FF-4070-B471-BB4AF12A724A)

    Press ESC in 5 seconds to skip startup.nsh, any other key to continue.
    Shell> fs0:

    fs0:\> dir
    Directory of: fs0:\

      01/02/13  07:50p <DIR>          2,048  ARM
      01/02/13  07:50p               15,904  InstallSctArm.efi
      01/02/13  07:50p                3,762  SctStartup.nsh
      01/02/13  07:50p              661,792  Shell_Full.efi
              3 File(s)     681,458 bytes
              1 Dir(s)

    fs0:\> InstallSctArm.efi
    add-symbol-file
    /home/olimar01/tianocore/Build/UefiSct/DEBUG_RVCTLINUX/ARM/SctPkg/Application/InstallSct/InstallSct/DEBUG/InstallSct.dll
    0xBF5F7240
        Loading driver at 0x000BF5F7000 EntryPoint=0x000BF5F8A45 InstallSct.efi

        Gather system information ...
          1: FS0: (Free Space: 123 MB)
          2: fsnt0: (Free Space: 0 MB)
          Space Required: 100 MB
        Input index of destination FS. 'q' to exit:1

        Backup the existing tests ...
    ```

7\. Start SCT in interactive mode

    fs0:\> cd SCT
    fs0:\SCT> SCT -u

![](ArmPkg-Sct-1.png "ArmPkg-Sct-1.png") Select the tests you want to
run ![](ArmPkg-Sct-2.png "ArmPkg-Sct-2.png")

... and finally press 'F9' to start the tests.
Once the tests have completed, SCT should return to its UI.
![](ArmPkg-Sct-3.png "ArmPkg-Sct-3.png")

Press 'ESC' to return to the main menu and select 'Test Report Generator
...' and save the report. By default, the report is located in the
directory SCT\Report\\ of the filesystem where you installed SCT.
![](ArmPkg-Sct-4.png "ArmPkg-Sct-4.png")

8\. To retrieve the result, you will need to mount your virtual MMC
card:

    sudo mount -o rw,loop=/dev/loop0,uid=`whoami`,gid=`whoami` /tmp/mmc.dat /tmp/fs
    cp /tmp/fs/SCT/Report/ComplianceTests.csv ~/

9\. Open the results with your favorite spreadsheet editor and identify
the failed tests

In our case we have one failed test:
![](ArmPkg-Sct-5.png "ArmPkg-Sct-5.png")

|  |  |
|----|----|
| Service\Protocol Name | GenericTest\EFICompliantTest |
| Index | 5.22.1.1.8 |
| Instance | 0 |
| Iteration | 0 |
| Guid | F6334F9B-B930-4ADB-A53B-76FA7B4C2762 |
| Result | FAIL |
| Title | UEFI-Compliant-Globally Defined Variable check |
| Runtime Information | /home/olimar01/tianocore/SctPkg/TestCase/UEFI/EFI/Generic/EfiCompliant/BlackBoxTest/EfiCompliantBBTestRequired_uefi.c:1322, Illegal Variable : PL031_TimeZone |
| Case Revision | 0x00010001 |
| Case GUID | 117C9ABC-489D-4504-ACDB-12AACE8F505B |
| Device Path | No device path |
| Logfile Name | RequiredElements_0_0_117C9ABC-489D-4504-ACDB-12AACE8F505B.log |

Generally the most important information is the 'Runtime Information';
in this case:
*/home/olimar01/tianocore/SctPkg/TestCase/UEFI/EFI/Generic/EfiCompliant/BlackBoxTest/EfiCompliantBBTestRequired_uefi.c:1322*.
This information will tell you where the error has been raised in the
SCT test code.

If we open the SCT source file at this location, we will find the
following code in the function CheckGloballyDefinedVariables():

        if (AssertionType == EFI_TEST_ASSERTION_FAILED) {
          StandardLib->RecordAssertion (
                         StandardLib,
                         AssertionType,
                         gEfiCompliantBbTestRequiredAssertionGuid009,
                         L"UEFI Compliant - Globally Defined Variables",
                         L"%a:%d, Illegal Variable : %s",
                         __FILE__,
                         (UINTN)__LINE__,
                         VariableName
                         );
          break;
        }

Additional information might be found in the Log file defined within the
'Logfile Name' column:

    ```text
        $ cat /tmp/fs/SCT/Log/GenericTest/EFICompliantTest/RequiredElements_0_0_117C9ABC-489D-4504-ACDB-12AACE8F505B.log

        (...)
    /home/olimar01/tianocore/SctPkg/TestCase/UEFI/EFI/Generic/EfiCompliant/BlackBoxTest/EfiCompliantBBTestRequired_uefi.c:901:Status
    - Success, Expected - Success

          GetDevicePathSize         : 00000000B6E2F054
          DuplicateDevicePath       : 00000000BF6DB521
          AppendDevicePath          : 00000000BF6DB535
          AppendDeviceNode          : 00000000BF6DB549

        VariableName: PL031_TimeZone  is not defined in the Spec

        UEFI Compliant - Globally Defined Variables -- FAILURE
        F6334F9B-B930-4ADB-A53B-76FA7B4C2762
    /home/olimar01/tianocore/SctPkg/TestCase/UEFI/EFI/Generic/EfiCompliant/BlackBoxTest/EfiCompliantBBTestRequired_uefi.c:1322,
    Illegal Variable : PL031_TimeZone

    Returned Status Code: Success

    RequiredElements: [FAILED]
      Passes........... 7
      Warnings......... 0
      Errors........... 1
    ------------------------------------------------------------
    UEFI 2.3.1
    Revision 0x00000018
    Test Entry Point GUID: 117C9ABC-489D-4504-ACDB-12AACE8F505B
    ------------------------------------------------------------
    Logfile: "\SCT\Log\GenericTest\EFICompliantTest\RequiredElements_0_0_117C9ABC-489D-4504-ACDB-12AACE8F505B.log"
    Test Finished: 01/01/1970  00:08
    Elapsed Time: 00 Days 00:00:12

    ------------------------------------------------------------
    ```

10\. The first step before debugging the failed test is to understand
the test itself and why it fails.

Our test walks through the entire variable list. It looks like one of
our driver defines the Globally Defined variable 'PL031_TimeZone'.

There are at least two ways to debug a failed SCT test either by doing a
code review of our UEFI firmware to try to fix it or the most
complicated way is to use a debugger to step through the test. For this
tutorial, we will choose the second option.

The difficulty of debugging UEFI is to load the symbols of dynamically
loaded EFI drivers and applications. We do not know in advance where the
UEFI binaries will be loaded. One way to do is to start the test a first
time to get the addresses where the binaries have been loaded and then
start again the test hoping the binaries will be loaded at the same
location ...

A more efficient way is to start SCT in interactive mode (command: \`SCT
-u\`) and load all the symbols of the active UEFI binaries with your
favorite debugger.
When SCT is started in interactive mode all the SCT test binaries are
(pre-)loaded in memory (to retrieve test information for the menu)
before the tests are started from the UI.

For this tutorial we will use DS-5 and its UEFI debugging scripts (see
[ArmPkg-Ds5](armpkg_ds5.md)).

For the Fast Model Versatile Express A9x4, after breaking the execution
of the model, type within the DS-5 'Commands' window (with \$EDK2_ROOT
the root of your EDK2 sources):

    source $EDK2_ROOT/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py -m (0x80000000,0x40000000) -a

And add a breakpoint in CheckGloballyDefinedVariables().

![](ArmPkg-Sct-6.png)
*ArmPkg-Sct-6.png*
Resume the execution of the Fast Model and start the SCT test to debug
from the SCT User Interface (by pressing F9). As soon as the function
where you have set the breakpoint is hit, DS-5 stops the execution.

You can now step though the SCT & UEFI Firmware code :-)
![](ArmPkg-Sct-7.png "ArmPkg-Sct-7.png")
