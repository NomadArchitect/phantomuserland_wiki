# What is going on

## Big fight about UI controls in 'controls' branch. 

Currently it  looks like this:

![](https://github.com/dzavalishin/phantomuserland/blob/controls/doc/images/phantom_screen_17_10_2019.png)

What's new:

* Button/menu item/radio/checkbox controls
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
As a bonus I added national keymaps support, currently with just one (Cyrillic,
of course) keymap, but now it is easy to add any language's keymap in a few minutes.

## True Type Fonts


