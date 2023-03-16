---
title: Speeding up Volatility with ramdisks
date: 2014-09-01
summary: Use ramdisks to speed up analysis on large memory dumps
tags:
  - memory forensics
  - volatility
  - tip
---

[Volatility](https://github.com/volatilityfoundation/volatility) is one of the
greatest memory forensic tools available out there. It's got tons of plugins,
it's open source, it's written in python, what's not to like? Plus,
they've just migrated to GitHub, which is awesome.

Volatility works on live memory (RAM) dumps. Most of the time, plugins such as
`pslist` can reveal interesting information just by scanning specific kernel
structures and walking lists. Plugins such as `psscan` take longer, since they
scan the whole memory dump looking for specific pool tags. There are other
read-intensive plugins such as `strings` that take even longer to run.

You can usually limit the time you spend running plugns by piping their output
to a text file (which is smart since the memory dump doesn't change in time
anyways). If you are developing - and therefore testing - your own plugins,
you'll have to run them every time, which can quickly become tedious if they
take ≈3 minutes to run.

### Ramdisks to the rescue

Ramdisks are like any other mounted device, only they map a portion of your live
memory to a directory on disk. It works just like any other device, only faster.
**Way faster**. Because their content is in RAM, any changes will be lost if
unmounted or if the workstation they're mounted on restarts, so make sure you
save your progress on a physical disk.

#### Mac OS X

Ramdisks are supported in Mac OS X natively. The following script was tested on
Mavericks 10.9.4:

```terminal
diskutil erasevolume HFS+ '[NAME]' `hdiutil attach -nomount ram://[SIZE]`
```

Where `[SIZE]` is the number of sectors of your new filesystem, and `[NAME]` is
the name you want to give to the new volume. To check your sector size:

```terminal
$ diskutil info / | grep "Block Size"
   Device Block Size:        512 Bytes
```

To mount a 8 GB (`8 * 1024 * 1024 * 1024 / 512 = 16777216` sectors) volume named
_RAMDISK_, you'd use:

```terminal
diskutil erasevolume HFS+ 'RAMDISK' `hdiutil attach -nomount ram://16777216`
```

To unmount, just eject the disk as you would with any USB key.

#### Linux

The following was successfully tested on Ubuntu 14.04 LTS:

```terminal
mkdir /mnt/ramdisk
mount -t [TYPE] -o size=[SIZE] [FSTYPE] [MOUNTPOINT]
```

Where:

- `[TYPE]` is the type of RAM disk to use; either tmpfs or ramfs.
- `[SIZE]` is the size to use for the file system. This understands units. (e.g.
  `1024m` for 1024 MB.)
- `[FSTYPE]` is the type filesystem you want to use; tmpfs, ramfs, ext4, etc.

To mount a 512 MB filesystem on `/mnt/ramdisk` you would use:

```terminal
mount -t tmpfs -o size=512m tmpfs /mnt/ramdisk
```

Unmount as any other device:

```terminal
umount /mnt/ramdisk
```

#### Windows

I haven't tested this, but a quick Google search gives the following utility:
http://www.tekrevue.com/tip/create-10-gbs-ram-disk-windows/.

### Speed gain

The speed gain you might experience may vary according to your system
configuration. I've had times when analysis carried out from a dump on a ramdisk
went up to **4x** as fast as on a typical hard-drive. The speed gain may also
vary according to which plugin you're using.

In my case, a `psscan` on a dump on the ramdisk took **3.1 seconds**, while the
same command on the same dump on a classical (non-SSD) hard-drive took **13
seconds**.

```terminal
(env-forensics)tomchop:malware tomchop$ time vol.py -f /Volumes/ramdisk/Windows\ XP\ Professional-130bb3ad.vmem --profile=WinXPSP2x86 psscan
[...]
real  0m3.124s
user  0m2.072s
sys   0m0.898s
```

```terminal
(env-forensics)tomchop:malware tomchop$ time vol.py -f Windows\ XP\ Professional-130bb3ad.vmem --profile=WinXPSP2x86 psscan
[...]
real  0m13.060s
user  0m2.102s
sys   0m0.964s
```

In this case the time gain was noticeable, but it may vary from setup to setup.
Seeing how RAM is cheaper than SSD drives, it's definitely worth trying.
