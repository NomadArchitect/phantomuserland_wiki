# Overview

Main task of Phantom's kernel is providing a persistent environment for a virtual machine.

Such an environment consists of two things: persistent memory (VM lives in it) and persistent objects connecting VM to kernel.

## Persistent memory

Persistent memory is, most of the time, quite usual virtual (paged) memory. On 32 bit Intel it takes upper 2GB of address space and is backed by Phantom disk.

What makes it to be persistent is snap shots that store list of pages (memory pages copied to disk) which belong
to consistent state of memory for some moment in time.

## SnapShot process

![SnapShot sequence](https://raw.githubusercontent.com/dzavalishin/phantomuserland/master/doc/diagrams/SnapShot_Sequence.png)

Kernel keeps on disk two last snapshots, and during snap shot process third one is being made:

* SnapShot N - previous, outdated, kept for case the current one is broken for some reason.
* SnapShot N+1 - current, the latest one which is complete. This one will be loaded if kernel will restart.
* SnapShot N+2 - new, being made right now, incomplete.

When N+2 one is finished, snapshot N is deleted. Now we have:

* SnapShot N+1 - previous, outdated.
* SnapShot N+2 - current.

