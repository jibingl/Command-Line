# Discovery to Windows Boot
## Convert MBR to GPT
Reference to [MBR2GPT.EXE](https://learn.microsoft.com/en-us/windows/deployment/mbr-to-gpt).  
**MBR2GPT.EXE** converts a disk from the Master Boot Record (MBR) to the GUID Partition Table (GPT) partition style without modifying or deleting data on the disk.  
#### Disk Prerequisites:
- The disk is currently using MBR
- There are at most 3 primary partitions in the MBR partition table
- Each volume has a drive letter assigned
#### Syntax:
```
MBR2GPT /validate|convert [/disk:<diskNumber>] [/logs:<logDirectory>] [/map:<source>=<destination>] [/allowFullOS]
```
#### Examples:
Validation only:
```console
X:\>mbr2gpt /validate /disk:0
MBR2GPT: Attempting to validate disk 0
MBR2GPT: Retrieving layout of disk
MBR2GPT: Validating layout, disk sector size is: 512
MBR2GPT: Validation completed successfully
```
Conversion a disk:
```console
X:\>DiskPart
DISKPART> list volume

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     F   CENA_X64FRE  UDF    DVD-ROM     4027 MB  Healthy
  Volume 1     C   System Rese  NTFS   Partition    499 MB  Healthy
  Volume 2     D   Windows      NTFS   Partition     58 GB  Healthy
  Volume 3     E   Recovery     NTFS   Partition    612 MB  Healthy    Hidden

DISKPART> select volume 2

Volume 2 is the selected volume.

DISKPART> list partition

  Partition ###  Type              Size     Offset
  -------------  ----------------  -------  -------
  Partition 1    Primary            499 MB  1024 KB
* Partition 2    Primary             58 GB   500 MB
  Partition 3    Recovery           612 MB    59 GB

DISKPART> detail partition

Partition 2
Type  : 07
Hidden: No
Active: No
Offset in Bytes: 524288000

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
* Volume 2     D   Windows      NTFS   Partition     58 GB  Healthy

DISKPART> exit

Leaving DiskPart...

X:\>mbr2gpt /convert /disk:0

MBR2GPT will now attempt to convert disk 0.
If conversion is successful the disk can only be booted in GPT mode.
These changes cannot be undone!

MBR2GPT: Attempting to convert disk 0
MBR2GPT: Retrieving layout of disk
MBR2GPT: Validating layout, disk sector size is: 512 bytes
MBR2GPT: Trying to shrink the system partition
MBR2GPT: Trying to shrink the OS partition
MBR2GPT: Creating the EFI system partition
MBR2GPT: Installing the new boot files
MBR2GPT: Performing the layout conversion
MBR2GPT: Migrating default boot entry
MBR2GPT: Adding recovery boot entry
MBR2GPT: Fixing drive letter mapping
MBR2GPT: Conversion completed successfully
MBR2GPT: Before the new system can boot properly you need to switch the firmware to boot to UEFI mode!

X:\>DiskPart
DISKPART> list disk

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online           60 GB      0 B        *

DISKPART> select disk 0

Disk 0 is now the selected disk.

DISKPART> list volume

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     F   CENA_X64FRE  UDF    DVD-ROM     4027 MB  Healthy
  Volume 1     D   Windows      NTFS   Partition     58 GB  Healthy
  Volume 2     C   System Rese  NTFS   Partition    499 MB  Healthy    Hidden
  Volume 3                      FAT32  Partition    100 MB  Healthy    Hidden
  Volume 4     E   Recovery     NTFS   Partition    612 MB  Healthy    Hidden

DISKPART> select volume 1

Volume 1 is the selected volume.

DISKPART> list partition

  Partition ###  Type              Size     Offset
  -------------  ----------------  -------  -------
  Partition 1    Recovery           499 MB  1024 KB
* Partition 2    Primary             58 GB   500 MB
  Partition 4    System             100 MB    59 GB
  Partition 3    Recovery           612 MB    59 GB

DISKPART> detail partition

Partition 2
Type    : ebd0a0a2-b9e5-4433-87c0-68b6b72699c7
Hidden  : No
Required: No
Attrib  : 0000000000000000
Offset in Bytes: 524288000

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
* Volume 1     D   Windows      NTFS   Partition     58 GB  Healthy
```

## Mount A Windows Image
Reference to [Modify a Windows image using DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/mount-and-modify-a-windows-image-using-dism?view=windows-11#mount-an-image).  
Mounted images are WIM, VHD, or FFU files that have their contents mapped to a folder.
#### Syntax:
```
DISM /Mount-image /imagefile:<path_to_Image_file> {/Index:<image_index> | /Name:<image_name>} /MountDir:<target_mount_directory> [/readonly] /[optimize]}
```
#### Example:
```console
C:\windows\system32>dism /Mount-image /imagefile:d:\sources\boot.wim /Index:1 /MountDir:c:\testmt\
Deployment Image Servicing and Management tool
Version: 10.0.19041.844
Mounting image
[==========================100.0%==========================]
The operation completed successfully.
```

## Unmount the Image
#### Syntax:
```
Dism /Unmount-image /MountDir:<target_mount_directory> {/Commit | /Discard}
```
#### Example:
```console
C:\windows\system32>dism /unmount-image /mountdir:c:\testmt\ /discard
Deployment Image Servicing and Management tool
Version: 10.0.19041.844
Unmounting image
[==========================100.0%==========================]
The operation completed successfully.
```
