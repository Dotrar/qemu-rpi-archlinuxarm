# qemu-rpi-archlinuxarm
ArchlinuxARM emulator using QEMU Easy setup script.

I made this script after messing around quite a bit on how to get the qemu working for rpi emulation of the latest archlinuxarm.

goals of this script is to be able to:

```console
git clone https://github.com/dotrar/qemu-rpi-archlinuxarm
cd qemu-rpi-archlinuxarm
./setup
./run
```

the setup file will install a desktop file, you can specify `-d` to output to console, thereby avoiding the desktop file install

### Manual process

The start command I use to start-up archlinux arm is: (note, some filenames differ)

```sh
#:: startup
qemu-system-arm \
 -kernel ./boot/kernel7.img \
 -cpu arm1176 \
 -M raspi2 \
 -dtb ./boot/bcm2709-rpi-2-b.dtb\
 -sd fs.img \
 -initrd ./boot/initramfs-linux.img\
 -append "root=/dev/mmcblk0p1 rw rootwait console=ttyAMA0,9600 console=tty1 selinux=0 plymouth.enable=0 smsc95xx.turbo_mode=N dwc_otg.lpm_enable=0 elevator=noop"
```

This process includes *no seperate boot partition*, If you want a seperate boot, you must change
`root=/dev/mccblk0p2` or where-ever your root partition now is.

the fs.img is a qemu-qcow2 image but might also work with raw images.
Built the fs.img to be 8GB, format with `mkfs.ext4`
then mounted with `qemu-nbd`

Copied the files from `wget http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-2-latest.tar.gz`

`bsdtar -xpf ArchLinuxARM-rpi-2-latest.tar.gz -C fs.mountpoint`

copy the kernel, dtb, and the initramfs out.

## Known issues

* Serial causes kernel panics, but can be the only way to get log-in, drivers crash but kernel keeps chugging.
* No Network install.
