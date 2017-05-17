Phantom HAL divides machine-dependent code in two parts: architecture dependent and board (specific kind of hardware) dependent.

See [[Machine dependent headers]], [[Toolchain setup]]

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

After that phantom_multiboot_main function must be called to init kernel.

phantom_multiboot_main inits:
* CPU (not MMU)
* interrupts
* boot console
* low level address map (physical memory management)
* kernel heap
* kernel arguments (command line)
* run constructors

by calling corresponding init functions and calls main() finally.

### arch_init_early() and board_init_early()

Called just after assembly code init is done to make architecture specific and board specific initializations. Can be empty.

### spinlock.c

Basic spinlocks:
* hal_spin_init/lock/unlock - as is
* check_global_lock_entry_count - called in sutuations when kernel expects that no spin lock is locked to check that

### paging.c

hal_page_control_etc - map or unmap virtual memory page according to parameters given

### sched.c

phantom_scheduler_request_soft_irq - must schedule soft IRQ for threads subsystem (```hal_request_softirq(SOFT_IRQ_THREADS);```) and cause any interrupt then.

