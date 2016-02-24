= Kernel Init/Stop subsystem =

To make kernel init/stop process more or less clear, Init/Stop subsystem is made.

== Init stages ==

Any subsystem can register up to three init functions.

```
#include <kernel/init.h>

static void early_init(void);
static void main_init(void);
static void late_init(void);

INIT_ME( early_init, main_init, late_init ); // Any one can be zero
```

=== Early init ===
**After this stage you must ensure that all public interfaces of your subsystem are callable**.
You can return failure for all of them, but not crash.

Very limited subset of kernel services is working. 

What is **NOT** working:

 * IRQ allocator
 * Paging
 * Timers and timed calls
 * Threads and locking. Though you **can** use spinlocks.
 * DPC (deferred procedure calls - threadlets/light threads)
 * Drivers
 * Network, of course.

=== Main init ===
**After this stage your subsystem must be mostly working**.
You can call other subsystems and integrate with them.

What is still **NOT** working:

* Video
* Phase 2-3-4 drivers
* Unix subsystem
* USB
* Disk IO
* Phantom native subsystem (persistent memory and virtual machine)

Network **IS** working at this stage, though.

=== Late init ===
**After this stage your subsystem must be completely working**.
You can call anything in kernel.



== Stop stages ==

```
static void early_stop(void);
static void prepare_stop(void);
static void final_stop(void);

STOP_ME( early_stop, prepare_stop, final_stop );
```

