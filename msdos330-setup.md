
# Installing MS-DOS and Windows/386

In the following, we will do the installation from virtual floppy disks, getting as close as possible to the real experience here. You'll have to imagine jockeying floppy disks and the screeching sound of the floppy drive while moving it's heads.

## Prerequisite

### MS-DOS installation floppy disk image

Version 3.30 will be great, as this vesrion of MS-DOS adds support for 1.44'' floppy drives and extended hard drive partitions. The installer fits on a single 360kB floppy disk!

Also, Windows/386 requires 3.30 as a minimum version of MS-DOS.

Version 3.31 was OEM-only, adding support for hard drives with up to 512MB. You may want to install this after a future hardware upgrade of your virtual retro PC.

Download e.g. from [winworldpc.com](https://winworldpc.com/product/ms-dos/3x), and unzip into a subdirectory (will assume *msdos330*).

## Install MS-DOS

Start up your virtual PC with the MS-DOS install floppy inserted:

```bash
qemu-system-i386 -machine type=isapc -cpu base -m 16M -k de \
                 -vga cirrus -rtc base=localtime \
                 -name "Virtual 386-PC" \
                 -boot order=ac \
                 -no-fd-bootchk \
                 -no-shutdown \
                 -monitor stdio \
                 -drive file=drive_c.img,driver=raw,index=0,media=disk \
                 -drive file=drive_d.img,driver=raw,index=1,media=disk \
                 -drive file=msdos330/Disk1.img,if=floppy,index=0,readonly=on
```

After confirming date and time by pressing Enter twice, you will be prompted by MS-DOS, which will have booted from the floppy disk.

Now, set up MS-DOS on the first hard disk. First, create a primary partion (cheat code is *1, 1, Y, Enter*):

```bash
A>fdisk
```

To make changes effective, fdisk will reboot your virtual PC. Now you can set up the file system on the hard disk and copy the system files onto it. MS-DOS 3.30 has a barely known setup utility for this (cheat: *Y, Y, Enter, DISK_C*):

```bash
A>select a: c:\dos 049 GR
```

Again, please observe the german localization here. For more options, check out the [MS-DOS documentation]() on page 4-140.

However, the localization will only be effective after MS-DOS has been set up on the hard disk and the virtual PC has been rebooted. Until then, take care to hit the right keys to enter the colons and the backslash in the above command.

To complete the installation of MS-DOS, switch off the virtual PC (`(qemu) quit`), and restart with the MS-DOS installation floppy disk removed (omit the last line in the above `qemu-system-i386` invocation command).

## Finalizing

Partition and format the second disc:
- `fdisk` *5, 1, 1, Y, Enter*
- `format d:` *Y, Enter*
- `label d:` *DRIVE_D*

Make the startup screen nicer and display currend working diectory in prompt. For this we will use the editor *edlin* which is shipped with MS-DOS. This editor is a very basic one, with line-oriented user interface. You first type in the line number you want to manipule and the one letter command (no letter means edit):

`edlin autoexec.bat`

*3, prompt $p$g, Enter*

*4d, 4d, 4d*

*1i, echo off, Ctrl-C*

*l* (list)

check if your yew autoexec.bat looks right

*e*

Now quit the emulator and go on to the next step.

# Next step

[Now it's time to install Windows/386!](https://github.com/retro-speccy/Windows386-playground/win211386-setup)