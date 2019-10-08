# Overview

Main task of Phantom's kernel is providing a persistent environment for a virtual machine.

Such an environment consists of two things: persistent memory (VM lives in it) and persistent objects connecting VM to kernel.

## Persistent memory

Persistent memory is, most of the time, quite usual virtual (paged) memory. On 32 bit Intel it takes upper 2GB of address space and is backed by Phantom disk.

What makes it to be persistent is snap shots that store list of pages (memory pages copied to disk) which belong
to consistent state of memory for some moment in time.

## SnapShot process

![SnapShot sequence](https://raw.githubusercontent.com/dzavalishin/phantomuserland/master/doc/diagrams/SnapShot_Sequence.png)
