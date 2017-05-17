Headers with machine-dependent stuff

## misc places, need to move to ARCH specific subtree too

### kernel/interrupts.h

```
#define         MAX_IRQ_COUNT
```

### setjmp.h

Jump buffer size

```
#define _JBLEN 10
```

## include/ARCH_NAME/arch

For example, include/e2k/arch/ for Elbrus 2000

### arch-flags.h

See kernel/boot.h

Parameters for ```ARCH_HAS_FLAG()``` and ```ARCH_SET_FLAG()``` macros, global info about CPU/HW abilities kernel needds to take in account.


### arch-config.h and board-$(BOARD)-config.h

These are forced to be included in every file during compilation, can be used to configure kernel for specific architecture. Supposed to modify global kernel/config.h defines ('casue included after).

Default board config file name is board-$(ARCH)_default-config.h 

### arch-cpu_state.h 

Defines ```struct cpu_state_save``` and corresponding macros to access it's fields from assembler.

Used in CPU state save during context (thread) switch.

### arch-elf.h 

Architecture dependent definitions for ELF file loader.

### arch-limits.h 

Just limits.h for this arch.

### arch-page.h 

Memory (MMU) page size, shift value and mask.

### arch-types.h 

Basic C types definitions.

### arch_endian.h 

Selects endianness.

