# Steps to get a virtual retro PC running with Qemu

To incarnate a virtual retro i386-based PC on your current PC (called the host), you will need an emulator. QEMU will be used in the following, which has to be installed first.

## Download and install on your host machine

### QEMU

i386 support is dropped from v8.0 on, make sure you get the latest v7.x:

* [Install Qemu v7.x](https://www.qemu.org)

## Setup virtual hard disks of the retro PC

We will put two fairly large (for 1986), 32 megabytes hard disks into the retro PC. 32MB is the maximum size MS-DOS 3.30, the operating system we will install later, can handle.

The contents of the hard drives will be saved into one big image file, holding the raw data as a real hard disk would do. QEMU will read the contents of that image file and emulate a hard disk.

We will start with empty drives C: and D:, fresh out of the box:

```bash
qemu-img create -f raw drive_c.img 32M
qemu-img create -f raw drive_d.img 32M
```

## Switch it on!

Now it's time to switch on your virtual PC for the first time. As we have not installed anything yet, the PC screen should just show some BIOS messages and a boot error. This will still be enough to test the steps above:

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

Note on localization: in the cmmmand above, *-k de* instructs QEMU to use a german keyboard layout. For other keyboard layouts, see the *-k language* section in the QEMU invocation [documentation](https://www.qemu.org/docs/master/system/invocation.html).

### QEMU monitor

In the terminal window where you have entered the above command QEMU will print a message showing some info on the running emulator and a prompt. At this prompt QEMU-specific commmands can be entered to manipulate the emulator.

If you are adventurous, check the [monitor documentation](https://www.qemu.org/docs/master/system/monitor.html) for more.

## Switch it off

Before beginning to install MS-DOS and Windows/386, let's switch the virtual PC off with a simple command at the monitor prompt:

```bash
(qemu) quit
```

# Next step

[Now it's time to install MS-DOS!](https://github.com/retro-speccy/Windows386-playground/blob/main/msdos330-setup.md)