## QEMU on Windows or Linux ##

```
cd $(PHANTOM_HOME)
cd run
./phantom.sh or phantom.cmd
```

This will start Phantom in a usual way - it will pick up prevoius start's snapshot if possible.

If you need (and you often will) to start 'from a green field', run phantom_clean.sh/cmd instead.

## QEMU on MacOS ##

phantom.sh:

```
USB="-usb -usbdevice mouse"
VIO="-drive file=vio.img,if=virtio,format=raw"
Q_PORTS="-parallel file:lpt_01.log  -serial file:serial0.log"
Q_NET="-net user -tftp tftp -net nic,model=pcnet"
Q_MACHINE=""
Q_DISKS="-boot a -no-fd-bootchk -fda img/grubfloppy.img -hda snapcopy.img -hdb phantom.img"
Q_VGA="-vga std"

rm serial0.log.old
mv serial0.log serial0.log.old

/opt/local/bin/qemu $Q_VGA $Q_KQ $Q_MACHINE $Q_PORTS  $Q_DISKS  $Q_NET $VIO $USB
```

## Real hardware ##

You will need some FAT/ext3/other disk GRUB can live on. Install GRUB on that disk.

Take files from run/fat/boot, place 'em to /boot directory on disk.

Have some other disk/partition formatted as Phantom disk. See `zero_ph_img.sh` to find out how.
In short, you just need to put a copy of Phantom disk superblock into the 16-th 4096 byte block.

`dd conv=nocreat conv=notrunc bs=4096 count=1 seek=16 if=img/phantom.superblock of=phantom.img`

Phantom sends kernel log into serial port, it can help to find out what's wrong if kernel dies.

See also [[Making bootable image with grub2]]