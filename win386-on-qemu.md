# Steps to get Windows/386 running on a virtual PC with Qemu

## Introduction

### What is Windows/386?

A flavour of Windows 2.x and the first Windows to use the protected mode features of the then new 386 processor. It was the first to offer parallel execution of applications in virtual 8086-mode offering 640k memory for each application, thus being the first version offering real usability.
It was pressed into production by Compaq to make use of the 386 processors introduced in their [new line of computers](https://dfarq.homeip.net/compaq-deskpro-386/) overtaking IBM still just selling 286-based systems. Microsoft was even using Compaq Deskpro 386 machines for development.

Further reading:
[Wikipedia](https://en.wikipedia.org/wiki/Microsoft_Windows_version_history) history
[YouTube](https://www.youtube.com/watch?v=noEHHB6rnMI) must see Microsoft ad, impatient viewers fast forward to 7:00

### But why?

To be able to check out [Windows/386 dark mode](github)!

## Prerequisites

### QEMU

* [Install CMake]https://cmake.org/install/
* [Install qemu]

### DOS

Although Windows/386 can be installed on a "modern" DOS (like MS-DOS 5), we will stick to the real experience here. Therefore we assume a PC built by Compaq and we will install an appropriate DOS version, even if comes without an installer. This makes installing DOS a little more cumbersome, but will give the real feeling!

1. MS-DOS 3.31 boot
2. Start fdisk
3. Create primary partition
4. reboot
5. format c: /s
6. md c:\DOS
7. copy *.* c:\dos
8. copy con c:\config.sys
9. country=044,,c:\dos\country.sys
lastdrive=z
shell=c:\dos\command.com c:\dos /p
10. ^Z
11. copy con c:\autoexec.bat
12. path c:\dos;c:\
mode con codepage select=850
keyb gr,,c:\dos\keyboard.sys
prompt $P$G
^Z
13. reboot
14. 



1. MS-DOS Compaq OEM installation floppy disk image
Version 3.30 will be great, as MS-DOS adds support for 1.44'' floppy drives and extended hard drive partitions by this version.
Version 3.31 was OEM-only, adding support for hard drives with up to 512MB.
E.g. [winworldpc.com](https://winworldpc.com/product/windows-20/windows-386)
2. Windows/386 installation floppy disk images
E.g. [winworldpc.com](https://winworldpc.com/product/windows-20/windows-386)

### How to change floppy disks

When installing programs which do not fit onto one floppy disk, users are occasionally prompted to change the floppy disk in the floppy disk drive. To accomplish this virtually follow the steps below.
Play attention to "insert" the right floppy, they are not always all required or in sequence.

1. Press `Ctrl-Alt-2` to start Qemu's monitor screen
2. Type `eject fda⏎`, then `change fda /path/to/new/floppy.img⏎`
3. Press `Ctrl-Alt-1` to return to the installation procedure on the virtual PC

## Installation procedure

To feel the real 1988 user experience, follow these steps:

### Create C: drive raw image

Let's first create a virtual hard drive. This will be the target of the following installation procedure. At the end of this procedure this image will hold a ready-to-go Windows/386 installation.
32MB is the maximum size in MS-DOS 3.30.

```bash
qemu-img create -f raw win386.img 32M
```

### Qemu command

As we assume an old PC, let's play the game as real as it gets and build a PC as one would have found on the most wealthy corporate desktops: a Compaq Deskpro 386 with M RAM, VGA, PC Speaker, and a mouse.

### Installing MS-DOS

1. fdisk
2. sys
3. copy content of first disk
4. copy content of second disk
5. copy content of third disk

### Restart the virtual PC

### Installing Windows/386

Start up DOS, put the first diskette into the virtual floppy drive and get ablazed by Microsoft Windows/386's flashy installer!

Fun fact: to select German localization, "West Germany" has to be selected in the menu (scroll down with cursor key, list does not fit onto screen and scrolling possibility is not obvious).
Those where the times in 1987, an installation of Windows in the [German Democratic Republic](https://en.wikipedia.org/wiki/East_Germany) was [prohibited](https://en.wikipedia.org/wiki/Coordinating_Committee_for_Multilateral_Export_Controls) (and very unlikely anyhow).

## Testing/using the installation

1. install app on second drive
2. make win386.img readonyly?

## Shortcut

If you just want to get going with Windows/386 right away and skip the installation procedure, download the completed image from [GitHub](retro-speccy).

## Yard

```bash
qemu-system-i386 -machine type=isapc -cpu base -m 16M -k de \
                 -soundhw pcspk -vga cirrus -rtc base=localtime\
                 -name Windows/386 \
                 -boot order=ac,menu=on \
                 -no-fd-bootchk \
                 -no-reboot -fda file=~/Downloads/COMPAQ-DOS331-DISK1.img \
                 -drive file=~/Projects/x86/images/win386.img,snapshot=on -no-shutdown

floppy=on,index=0,if=floppy,readonly=on -no-fd-bootchk 
splash=sp_name.bmp800x600,splash-time=3000

qemu-system-i386 -machine type=isapc -cpu x86 base -m 16M -k de \
                 -soundhw pcspk -vga cirrus -localtime \
                 -name Windows/386 \
                 -boot order=ac,menu=on, \
                 -no-reboot -drive file=floppy=on,index=0,if=floppy,readonly=on -no-fd-bootchk \
                 -drive file=win386.img,snapshot=on -no-shutdown
```

https://en.wikibooks.org/wiki/QEMU
https://qemu-project.gitlab.io/qemu/system/invocation.html#hxtool-0
https://www.theblackzone.net/posts/2018/msdos-in-qemu.html
https://gunkies.org/wiki/Installing_MS-DOS_on_Qemu
https://fosspost.org/use-qemu-test-operating-systems-distributions/?__cf_chl_jschl_tk__=5bfca25c9aef7a89c98a587b77ba69ffb375033b-1619041478-0-AYzRaYZ7v_zaR6AX7P40T4sAD0D7ewLJn7jzA_VcN49ME372VMT-gx_WC5SDMTOnb5n59Hbddo84ZNr-OwsQidjblrH9uWU6oBdoF0kMCiop8BOeF2gIKpoRDyd_iFySFGjk7ht6OsMi5k7NeGi6Y1ifCrrMcB_V4zF4gUyWIwmioqnIwWt5YxPNfmFuTD4DyDcjXn1FZSj8MMdoHKhf1iy2pn31PGYcBnns-f9MJtXiv7KzOrLjen8OZ_0mQDukyCXWKJnjGXY8BsxTJrySYz8kBUTF3K4WDRIVFeHp9CR1zSt9nIjsupGu2Czq65lIOMZNMX3-qEzlaPqY_-koFZ27bvq6rNTxaBe8BJnfJBOwYMDW_RlqMgTl5iyb99aqnmb0XfmljHJlLOu2nwY4RbkJh2U67q64Im_YobjgMLe1LcrxKq7DgTRD84pj2bw46g
