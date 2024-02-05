
# Installing Windows/386

In the following, as with MS-DOS, we will do the installation from virtual floppy disks, getting as close to the real experience here. You'll again have to imagine jockeying floppy disks and the screeching sound of the floppy drive while moving it's heads.

## Prerequisites

### 1. Windows/386 installation floppy disc images

Get the latest version of Windows/386, 2.11.

Download e.g. from [winworldpc.com](https://winworldpc.com/product/windows-20/windows-386), and unzip into a subdirectory (we will assume *win211386*).

## Install Windows/386

Start up your DOS-PC, put the first diskette into the virtual floppy drive and get ablazed by Microsoft's flashy Windows/386 installer!

We will start up the virtual PC without any floppy disc inserted. The Windows/386 install floppies are not bootable, but are intended to be be run from MS-DOS. The PC would attempt to start from the floppy if it would have been inserted before startup.

```bash
qemu-system-i386 -machine type=isapc -cpu base -m 16M -k de \
                 -vga cirrus -rtc base=localtime \
                 -name "Virtual 386-PC" \
                 -boot order=ac \
                 -no-fd-bootchk \
                 -no-shutdown \
                 -monitor stdio \
                 -drive file=drive_c.img,driver=raw,index=0,media=disk \
                 -drive file=drive_d.img,driver=raw,index=1,media=disk
```

```bash
C:\>a:
A:\>setup
```

1. disk: Setup, Displays 1
2. disk: Displays 2, Utilities 1
3. disk: Utilities 2, Fonts
4. disk: Applications

### How to change floppy discs

When installing programs which do not fit onto one floppy disk, users are occasionally prompted to change the floppy disk in the floppy disk drive. To accomplish this virtually follow the steps below.
Play attention to "insert" the right floppy, they are not always all required or in sequence.

1. Press `Ctrl-Alt-2` to start Qemu's monitor screen
2. Type `eject fda⏎`, then `change fda /path/to/new/floppy.img⏎`
3. Press `Ctrl-Alt-1` to return to the installation procedure on the virtual PC

### Restart the virtual PC

To make changes effective, reboot your virtual PC by entering the following at the QEMU monitor prompt:

```bash
(qemu) system_reset
```

### Installing Windows/386

The Windows installer will guide you through the setup process.

When selecting a mouse, select first MS Mouse. If you cannot move the mouse pointer later when trying Windows/386, restart the installation process and select and other type of mouse.

*Fun fact:* to select German localization, `West Germany` has to be selected in the menu (scroll down with cursor key, list does not fit onto screen and scrolling possibility is not obvious).
Those where the times in 1987, an installation of Windows in the [German Democratic Republic](https://en.wikipedia.org/wiki/East_Germany) was [prohibited](https://en.wikipedia.org/wiki/Coordinating_Committee_for_Multilateral_Export_Controls) and very unlikely anyhow.

## Next step

Use your retro PC (not yet ready)
