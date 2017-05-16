**caveat emptor**: grub2 currently not supported by Phantom OS. Fixes should be made soon.
Also, currently there is disk reading troubles with running Phantom using modern QEMU and grub2. 

Utility **grub-mkrescue** generates a bootable GRUB rescue image. It is shipped with grub package, and requires mformat (comes with mtools package) and xorriso tools.

grub-mkrescue also allows to pack anything along with image, by specifying directory, containing required files (see example below). This allows not only creating useful rescue images for special cases, but also creating bootable live images.

By default, grub2 loads configuration from _/boot/grub/grub.cfg_
So, by creating folder named, for example, _image_, and putting this file and folder hierarchy in it, rescue grub2 image with custom configuration can be created:
`grub-mkrescue -o image_name.iso image`
Where **image_name.iso** - name of image created by utility, and **image** is folder with desirable additional image content. Argument **-o** allows to specify image name.

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

Please note: If you are putting fat folder into image, please comment line `set root='(hd0,msdos1)'`, else replace hd0 with drive and msdos1 with partition, where fat is mounted or it's content is located. You also don't need `boot` line.

Also note that phantom.img or it's equivalent is required somewhere in the machine, system won't work with it.

To learn more about grub2 and it's configuration, please refer to [grub2 documentation](https://www.gnu.org/software/grub/manual/grub.html).