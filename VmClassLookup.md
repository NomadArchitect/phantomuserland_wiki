## Introduction ##

To load some class kernel calls user code. User code supposed to find class by connecting to network repository (which is not ready yet for now), or by some other means (distribution disk, for example). Actually, there's just one way to bring class to system: kernel native class loader. In future it will be used for bootstrap only, to bring initial set of classes to OS, but for now it is all we have.

## Initial kernel class loader ##

Kernel class loader can load class from:

* Class bundle brought by multiboot loader as kernel module. See `run/fat/boot/classes` file, made in `plib/sys`.
* From FAT disk `/class` directory, containing .pc files with compiled Phantom VM classes. See `run/fat/class/*.pc`. This way it is simpe to debug Phantom userland code - compiler can put a copy of binary to `run/fat/class/` for kernel to pick up.

## TODO ##

Some way for kernel to reload class implementation runtime.

## Questions ##

Class signing?
