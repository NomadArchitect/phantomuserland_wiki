Phantom HAL divides machine-dependent code in two parts: architecture dependent and board (specific kind of hardware) dependent.

What architecture and board we build for is defined in ```local-config.mk``` (see ```local-config.mk.in``` for ref)

```
ARCH=e2k
BOARD=some_elbrus_board_name
```

Machine dependent code directories:

```
include/e2k
oldtree/kernel/phantom/e2k # arch (CPU) dependent code comes here
oldtree/kernel/phantom/e2k/some_elbrus_board_name # specific hardware conf dependent code comes here

phantom/dev/e2k - specfic arch drivers come here

phantom/libc/e2k - asm code for libc comes here

phantom/libkern/e2k - see sibling folders
phantom/libphantom/e2k - see sibling folders
phantom/threads/e2k - context switch, cpu state, etc

phantom/Makeconf-e2k - toolchain setup (names of binaries to build and link)
oldtree/kernel/Makeconf-e2k - toolchain
```

## oldtree/kernel/phantom/ARCH

### entry.S - kernel entry point and basic hardware init

Must init hardware to the state when C code can be run.

After that C function must be called to init kernel - see phantom_multiboot_main for reference.

Must init:
* CPU (not MMU)
* interrupts
* boot console
* low level address map (physical memory management)
* kernel heap
* kernel arguments (command line)
* run constructors

and call main() finally

