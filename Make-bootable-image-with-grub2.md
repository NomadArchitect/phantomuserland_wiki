# Packing 'fat' folder into bootable image with grub2

**caveat emptor**: currently, this images only guaranteed to work under QEMU. We still look forward to launch Phantom OS on real hardware with grub2 - any help appreciated!

Utility **grub-mkrescue** generates a bootable GRUB rescue image. It is shipped with grub package, and requires mformat (comes with mtools package) and xorriso tools.

grub-mkrescue also allows to pack anything along with image, by specifying directory, containing required files (see example below). This allows not only creating useful rescue images for special cases, but also creating bootable live images.

By default, grub2 loads configuration from _/boot/grub/grub.cfg_
So, by creating folder named, for example, _image_, and putting this file and folder hierarchy in it, rescue grub2 image with custom configuration can be created:

```
grub-mkrescue -o image_name.iso image
```

Where **image_name.iso** - name of image created by utility, and **image** is folder with desirable additional image content. Argument **-o** allows to specify image name.

You might want to add some grub modules. To do so, add `--modules="..."` key to your command. Currently, I use following command to create bootable Phantom OS image:

```
sudo grub-mkrescue -o ./phantom_boot.img --modules="font gfxterm echo reboot usb_keyboard multiboot fat ls cat ext2 iso9660 reiserfs xfs part_sun part_gpt part_msdos" ./fat/
```

Note that sudo is required, and do 

`sudo chown $USER phantom_boot.img`

afterwards to have full access to image.

To create bootable Phantom OS image with grub2, we put custom grub.cfg (creation of one will be explained bellow) in fat/boot/grub/, and create use grub-mkrescue with folder fat as argument.

Unlike grub-legacy (older grub), menu list is specified in grub.cfg rather than separate menu.lst. 
Here is an example of menu entry, ported from original menu.lst:

```
menuentry 'phantom ALL TESTS' {
	set root='(hd0,msdos1)'
	multiboot /boot/phantom -d=20 -- -test all
	module /boot/classes
	module /boot/pmod_test
}
```

It is worth mentioning that on real hardware, grub won't load kernel without following lines in grub.cfg:

```
# A little of additional info
set pager=1

if [ "${grub_platform}" == "efi" ]; then
	echo "Loading EFI modules"
	insmod efi_gop
	insmod efi_uga
elif [ "${grub_platform}" == "pc" ]; then
	echo "Loading BIOS modules"
	insmod vbe
fi

insmod font
	
if loadfont ${prefix}/unicode.pf2
then
   	insmod gfxterm
   	set gfxmode=auto
   	set gfxpayload=keep
   	terminal_output gfxterm
fi
```

It is also worth mentioning that it still doesn't load anything on real hardware, but this removes any error messages from grub, so, it is still something.

Please note: If you are putting fat folder into image, please comment line `set root='(hd0,msdos1)'`, else replace hd0 with drive and msdos1 with partition, where fat is mounted or it's content is located. You also don't need `boot` line.

Also note that phantom.img or it's equivalent is required somewhere in the machine, system won't work with it (but still will load GUI, it will be a good sign)

To learn more about grub2 and it's configuration, please refer to [grub2 documentation](https://www.gnu.org/software/grub/manual/grub.html).