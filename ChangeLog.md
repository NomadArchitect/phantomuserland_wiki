# What is going on

## Big fight about UI controls in 'controls' branch. 

Currently it  looks like this:

![](https://github.com/dzavalishin/phantomuserland/blob/controls/doc/images/phantom_screen_17_10_2019.png)

What's new:

* **Button/menu item/radio/checkbox controls**
* **Context menu for windows and controls**
* Groups of controls (radio, but can group usual buttons too)
* Alpha blending for bitmaps - need it to paint control backgrounds
* Controls generate events and can call callbacks

## New life for PS/2 

Old PS/2 keyboard and mouse driver was quite quick and dirty. It relied
on ability of keyboard hw (or QEMU) to generate keyboard codes for very
old PC XT keyboard.

This mode is unsupported now, we need to understand new keyboard codes.

Hoping that uOS keyboard driver is able to support new keyboard codes I
ported it to Phantom. Just to find that this driver uses PC XT keyboard mode too.

So I had to rewrite it to work win second generation of keyboard scan codes.
As a bonus I added **national keymaps support**, currently with just one (Cyrillic,
of course) keymap, but now it is easy to add any language's keymap in a few minutes.

## True Type Fonts

It was apparently not so hard to port in **truetype rendering engine**. The only way
now to bring font is to embed it as a file image into the kernel, but later it will
be possible to bring it with object land class or download from network and store in
a persistent object. Last way seems to be the best for me.

## SVGA drivers

Lot of cleanup and rework on SVGA drivers. As a result:

* Cirrus SVGA (one of the best in QEMU) supports **hardware cursor**.
* VMWare SCGA code fixed, supports hardware cursor and even **hardware blit/fill**. Use of blit is not ideal though.
* Have Parallels virtual SVGA driver too. Thanx to Tormasov and unknown guys from Parallels.

## Object land

* setImmutable() - now can tell object to become immutable. Step to being able to 
send objects over the network from one Phantom to other.
* Object references were for long time structures with pointer to object itself and its interface (VMT). It is decided that it is impossible to maintain integrity in a multithreaded and SMP environments, so now it is just a simple pointer. It will make complete support of interfaces to be bigger problem, but I already know the solution.
* VM cast instruction is not a no-op anymore, it checks if object really belongs (has in parents) to a given type.
* Class loader code is using a cache (.internal.directory object) to speed up class access and prevent situation of duplicate classes.
* Now have instruction to check for correct number of args for method. Speeds up a bit.
* Reserve space on stack VM operation.
* Static_invoke to run parent's constructor.

## Phantom language compiler

* Generate .java stubs for phantom classes to be able to access 'em from Java. Step to support Java as userland language. If we want to support Python too, have to generate .py stubs then?
* Args for if/while/for cast expression to integer.
* Fixed storage of long/float/double to be in numeric, not object stack. Speed up.
* Generate constructor if no one. Call parent constructor.
* Constructor parameters

## SnapShot subsystem

Very big work on rewriting synchronization and blocking system calls. Quite happy with the results - test
system spent more than 400 snapshots and 100 abrupt reboots with no degradation at all.

Details are under the [[PersistentMemory]] article. If you need more info, drop me an issue.

Minor fix - kernel now can switch to previous snapshot if last one is broken. Reliability.

## Userland and around

As a result of a new snap sync architecture decided to kill .internal.connection stuff and return to 
more of internal classes as a way to communicate between userland and kernel.

Added new and good **JSON parser** and a connection to it from object land. Now can do a HTTP request, get
and parse returned JSON. As an example use Yandex weather API.

Minor object land stuff:

* atoi/atol
* object land mutex/cond, not really tested though


