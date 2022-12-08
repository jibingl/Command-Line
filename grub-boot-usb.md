# _GRUB_ Sets to Boot from USB

First make sure you have secure boot disabled from the firmware settings. Then follow the following steps:
- 1️⃣ Press _c_ when in grub menu to open command line.
- 2️⃣ Type `ls` to list all partitions in all hard drives.
- 3️⃣ Type `ls (hd0,gpt1)` to confirm the USB drive correctly.  
> Inplace of (hd0,gpt1) type the address of first partition of usb disk e.g: (hd1,gpt1) or (hd2,gpt1). According to output of ls command.
- 4️⃣ Note the UUID of you USB drive. Then seting the USB as bootable OS source by typing a set of commands.
- 5️⃣ Select the efi file to boot from.
- 6️⃣ Reboot.

Example:
```grub
grub>ls 
(hd0) (hd0,gpt1) (hd1) (hd1,gpt8) (hd1,gpt7) (hd1,gpt6) (hd1,gpt5) (hd1,gpt4) (hd1,gpt3) (hd1,gpt2) (hd1,gpt1)

grub>ls (hd0,gpt1) 
Partition hd0,gpt1: Filesystem type fat - Label `CES_X64FREV`, UUID 4099-DBD9 Partition start-512 Sectors...

grub>insmod part_gpt
grub>insmod fat
grub>insmod search_fs_uuid
grub>insmod chain
grub>search --fs-uuid --set=root 409-DBD9

grub>chainloader /efi/boot/bootx64.efi

grub>boot
```
[Reference Source](https://askubuntu.com/questions/947409/is-there-a-way-to-boot-from-usb-through-grub-menu)
