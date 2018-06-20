# How to run and update sdcard images in an ARM emulator on Arch Linux

## Installing all software needed for emulation

Install the following (together with dependecies) from the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository#Installing_packages)

* [binfmt-support-git](https://aur.archlinux.org/packages/binfmt-support-git/)
* [binfmt-qemu-static](https://aur.archlinux.org/packages/binfmt-qemu-static/)
* [qemu-arm-static](https://aur.archlinux.org/packages/qemu-arm-static/)
* [multipath-tools](https://aur.archlinux.org/packages/multipath-tools/)

Example (using [yaourt](https://aur.archlinux.org/packages/yaourt/) but you could go with [pakku](https://aur.archlinux.org/packages/pakku/), [pacaur](https://aur.archlinux.org/packages/pacaur/), [naaman](https://aur.archlinux.org/packages/naaman/) or whatever):

```console
$ yaourt -S --noconfirm --needed binfmt-support-git binfmt-qemu-static qemu-arm-static multipath-tools
```

Install scripts and update using:
```console
# sudo pacman -S --needed arch-install-scripts
# sudo update-binfmts --enable qemu-arm
```


## Mounting a backup of an SD-card and copying out the raw binary diskimage

There is a document on how to backup and clone SD-cards to images inside compressed folders. This guide will assume you have an archived image inside a SquashFS SQF-container obtained for example from a backup folder on Google Drive.

### Mount an SQF archived image to a subfolder and copy out the raw disk image

* You should probably make yourself a working directory and chdir into it, example: `mkdir virtual-archlinux-rpi; cd virtual-archlinux-rpi
* Download the image.sqf (for example "2018-06-15-post-Albanus.sqf")
* Create a mountpoint directory, mount the SQF container and copy out the actual raw image from inside the container, example:

```console
$ mkdir sqfmnt
$ sudo mount image.sqf sqfmnt
$ cp -v sqfmnt/sdcard.dd sdcard.dd
```

## Mounting the raw binary diskimage

## Set up loop devices pointing to partitions inside the raw diskimage

* Create loop devices using *kpartx* on the disk image like in the following example:

```console
$ sudo kpartx -a -v sdcard.dd
add map loop0p1 (254:0): 0 204800 linear 7:0 2048
add map loop0p2 (254:1): 0 30242816 linear 7:0 206848
```

### Mount loop devices into mountpoint
* If needed, create a mountpoint for the filesystem in the image with `mkdir archlinux-rpi`

```console
$ sudo mount /dev/mapper/loop0p2 archlinux-rpi
$ sudo mount /dev/mapper/loop0p1 archlinux-rpi/boot
```

## Copy the statically linked emulator binary into the mounted filesystem

```
$ sudo cp /usr/bin/qemu-arm-static archlinux-rpi/usr/bin
```

## Chroot inside the mountpoint using ARM emulation

```console
$ sudo arch-chroot archlinux-rpi /bin/bash
[root@sailbot /]# uname -a
Linux sailbot 4.17.2-1-ARCH #1 SMP PREEMPT Sat Jun 16 11:08:59 UTC 2018 armv7l GNU/Linux
[root@sailbot /]#

```

