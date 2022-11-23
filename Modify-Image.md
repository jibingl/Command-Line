#
## Mount A Windows Image
Reference to [Modify a Windows image using DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/mount-and-modify-a-windows-image-using-dism?view=windows-11#mount-an-image)
### Syntax:
```
DISM /Mount-image /imagefile:<path_to_Image_file> {/Index:<image_index> | /Name:<image_name>} /MountDir:<target_mount_directory> [/readonly] /[optimize]}
```
### Example:
```
dism /mount-image /imagefile:d:\sources\boot.wim /index:1 /mountdir:c:\testmt\
```

## Unmount the Image
### Syntax:
```
Dism /Unmount-image /MountDir:<target_mount_directory> {/Commit | /Discard}
```
### Example:
```
dism /unmount-image /mountdir:c:\testmt\ /discard
```
