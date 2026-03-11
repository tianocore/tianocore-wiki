# Tool Information

This is a table of all of the tools that are part of the [EDK II BaseTools](edk2_buildtools.md).

To build these tools see [BuildTool Setup Guide](../environment-setup/buildtool_setup_guide.md).

| Tool Name | Tool Description |
| --- | --- |
| BootSectImage.exe | Parses the content of input file with Filename and prints BPB information to the screen. When patch option is specified it will patch BPB information using information from an input file or MBR. |
| BPDG.exe | Used to patch VPD binary data files. |
| build.exe | The master command line (CLI) tool that provides a single command for selecting various build options. |
| ECC.exe | Checks an EDK II Package directory for coding style. |
| EfiLdrImage.exe | Combines PE files into one with EFI loader header. |
| EfiRom.exe | Is used to build an Option ROM image from UEFI PE32 file(s) and/or legacy option ROM images that conform to PCI 2.3 or PCI 3.0 specifications for Option ROM layout. |
| GenBootSector.exe | Reads boot sector data of a drive into file or writes the boot sector data to a drive `from a file.` |
| GenCrc32.exe | Generates a CRC32 value when encoding the input file, then puts the calculated CRC32 value into the output file header. |
| GenDepex.exe | Parses the input dependency expression string or the preprocessed DSX file to generate the binary PI dependency expression according to module type. |
| GenFds.exe | Generates the Ffs, Fv, FD and Section data depending on the selected command line options. |
| GenFfs.exe | Generates FFS files for inclusion in a firmware volume. |
| GenFV.exe | Generates a PI firmware volume image or a UEFI capsule image from the PI firmware files or the binary files, which conforms to the firmware volume image format defined in PI specification or UEFI capsule image format defined in UEFI specification. |
| GenFw.exe | Processes a PE32 image to get image data or image file. |
| GenPage.exe | Is composed of two parts: the page table part, placed at the offset specified from option, and the non-page table part which is placed at the beginning of the output file. |
| GenPatchPcdTable.exe | Searches the image map file to find every patchable PCD name and its real address, then parses the binary EFI image to get each section name and address, and calculates PCD offset in the binary EFI image and writes it into the output file. |
| GenSec.exe | Generates valid EFI_SECTION type files, which conform to the firmware file section defined in the PI specification, from PE32/PE32+/COFF image files or other binary files. |
| GenVtf.exe | Generates the Boot Strap File (AKA Volume Top File, or VTF) for IPF images. |
| LzmaCompress.exe | Encodes or decodes files with LZMA encode or decode algorithm. |
| PatchPcdValue.exe | Sets the specific value into the binary image according to the input PCD offset and type. |
| Rsa2048Sha256GenerateKeys.exe | Generates signing keys. |
| Rsa2048Sha256Sign.exe | Used to sign an efi image |
| Split.exe | Creates two Binary files either in the same directory as the current working directory or in the specified directory. |
| TargetTool.exe | Prints current build setting, clear current setting, or modify the current setting in target.txt. |
| TianoCompress.exe | Encodes or decodes files with EFI extension encode or decode algorithm. |
| Trim.exe | Processes the preprocessed file by Compiler to remove the unused content to generate the file to be processed further by EDKII tools. |
| UPT.exe | Create, install or remove a UEFI Distribution Package. |
| VfrCompile.exe | Parses the preprocessed UEFI and Framework VFR file to generate UEFI IFR opcode table, Binary Data and IFR listing file. |
| VolInfo.exe | Displays the contents of a firmware volume residing in a file for informational purposes. |
