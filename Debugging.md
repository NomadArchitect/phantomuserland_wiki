# Debugging

## pvm_test + gdb

```
cd phantom/vm
gdb pvm_test
```

Can be useful to debug virtual machine.

## QEMU + gdb

When running kernel in QEMU, it is possible to connect to it with gdb. 

If you have any questions regarding QEMU configuration, please refer to [manual](https://qemu.weilnetz.de/doc/qemu-doc.html) first.

First of all, you need to run gdb server in QEMU. To do so, add following key to your QEMU configuration:
(this command is already present in _phantom_new.sh_ script)

```
-gdb tcp::1234
```

Replace '1234' with the desired port of your choice.

Please note that on Windows, older versions of QEMU (around 0.15) are incompatible with any kind of cygwin gdb. On Ubuntu, no such problem is present, with any version of QEMU.

After you launched QEMU, use following commands to run gdb:

```
cd $PHANTOM_HOME
cd run/fat/boot/
gdb phantom
(gdb) target remote localhost:1234
(gdb) set scheduler-locking on
(gdb) break multiboot.c:261
```

Note that last three commands are given for example - you might want to set breakpoint in other file, line, etc. `set scheduler-locking on` pauses all threads on breakpoint.

After `target remote` command, QEMU must pause. When you want to continue, write `continue` command in your gdb session.

Please note that currently `print` command works wrong due to compilation configuration. If you want to see the value of your variable, please use 'call printf(...)' command.

Old instructions is here:

```
cd oldtree/kernel/phantom
# make sure gdb can execute .gdbinit here
gdb
```

gdb will load symbols and connect to running kernel.

Please note macros in '''gdbmacros''' file (oldtree/kernel/phantom).

### PDB Phantom Debugger

Work in progress.

There is going to be an Java application in tools/pdb, which can connect to running '''pvm_test''' 
(and should be able to connect to running kernel also) and let one to browse persistent 
memory objects.

See '''phantom/vm/gdb.c''' for connector.

### Debug functions in kernel

`pvm_scan_print_subtree( pvm_object_t start, int max_depth )` - find out ref counts on object tree

`debug_catch_object()` - catch object by address on alloc or free.

`pvm_memcheck()` - check persistent allocation consistency.

`alloc_print_arenas()` - print alloc arenas info

### Debug tools in kernel

