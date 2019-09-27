# Blocking system calls

See also: [[SnapSync]], [[ObjectKernelConnector]], [[KernelObjectInterface]], [[PhantomArchitecture]]

## Problem

Virtual machine code can not call long running or blocking kernel code. It will
break snapshots: it is impossible to save state of C code in kernel. On restart
only state of object land memory is restored, and VM threads continue to run from
last position.

## Solution

`.internal.connection` class provides `block()` method which is only one that can block 
in Phantom kernel. Here is the way it works:

* VM program calls `block()` method, which is in internal class `.internal.connection`.
* Method code consists of one instruction - `sys <syscall number>`
* Instruction causes VM interpreter to call vm_syscall_block() function in kernel (snap_sync.c)

The vm_syscall_block() in order:

* Reads and stores method args from stack.
* Restores stack state as it was before `sys` instruction was executed.
* Moves VM IP back to the beginning of `sys` instruction
* Revokes access to object memory, letting snapshots to happen
* Calls kernel code to execute required function

Now two scenarios are possible.

**Kernel is running uninterrupted until kernel code is done and returned.** 

* Access to object memory is gained, preventing snapshots.
* Stack is cleared of `sys` arguments
* IP is moved forward to skip `sys` instruction
* Result object is pushed as a return value of `sys`
* Control is returned to VM interpreter, letting it to execute next instruction

**Kernel made a snapshot, was uninterrupted and reboot happened.**

* VM interpreter restarts thread and finds itself in front of a `sys` instruction, which caused all that.
* Execution of `sys` instruction is restarted from scratch.

