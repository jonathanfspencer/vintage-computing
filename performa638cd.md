# Apple Performa 638CD
Luckily, this one worked perfectly when I got it.  It had the [standard specs from EveryMac](https://everymac.com/systems/apple/mac_performa/specs/mac_performa_638cd.html), but I have, so far, upgraded to 36 MB RAM and replaced the hard drive with an 8 GB CompactFlash.

## Adding Applications
Trying to get additional software on the thing is a bit of a challenge.  I do not have any other Apple devices with floppy drives.  The only machine I _do_ have with a floppy drive is my [PII 350 running Windows 2000](e4200.md), which does not exactly know how to write HFS formatted floppies.  Even getting software onto that is a bit of pain, but at least I can transfer things to it with a USB flash drive.  Not so with the Performa.

The most reliable way I have found so far to transfer files (from, say, [Macintosh Garden](https://www.macintoshgarden.com/)) to the Performa is shutting it down, pulling the drive, connecting it to a machine running Ubuntu 20.04 (other distros probably work), and using the `hfsutils` package.

1. Open a terminal and run `sudo apt install hfsutils` to get the package.
2. Hook up the hard drive to your system somehow.  I replaced the hard drive with a CompactFlash card in an IDE->CF adapter, so I use a USB CompactFlash reader.  You can also use a USB->IDE adapter in this instance, since the Performa 630 series used IDE drives.
3. The "Disks" app in Ubuntu will tell you the path of your drive with minimal hassle, but there is probably some way to use `dmesg`.  My path is generally `/dev/sdc3` for the partition I want to use.
4. On my machine I need to be root to use `hfsultils`.  To mount the drive, make sure you do not have it mounted elsewhere and run `sudo hmount /dev/sdc3` (or whatever your partition's path is).
5. The command to copy files takes some getting used to.  I suggest changing to the directory from which you want to copy in your terminal first.  If you want to copy a file called "disk1.sit" into the root of an HFS partition called "Macintosh," you would run `hcopy disk1.sit -a Macintosh:`.  The `-a` flag tells `hcopy` to automatically determine the best way to copy the file (there are several binary and ascii modes from which you can choose).  The colon at the end there is the folder delimiter. Another example could look like `Macintosh:Applications:"Microsoft Office"` (note the quotes around paths with spaces).
6. There are `hfsutils` equivalents for most of your common filesystem creation tools, such as `hdir`, `hrmdir`, `hmkdir`, and so on.  Deleting folders is a pain, since you cannot delete non-empty folders.
7. Once you are finished, use `humount` to unmount your HFS drive.
