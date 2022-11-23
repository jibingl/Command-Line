# Discovery to Windows Boot
## Convert MBR to GPT
Reference to [MBR2GPT.EXE](https://learn.microsoft.com/en-us/windows/deployment/mbr-to-gpt).  
**MBR2GPT.EXE** converts a disk from the Master Boot Record (MBR) to the GUID Partition Table (GPT) partition style without modifying or deleting data on the disk.  
#### Disk Prerequisites:
- The disk is currently using MBR
- There are at most 3 primary partitions in the MBR partition table
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
