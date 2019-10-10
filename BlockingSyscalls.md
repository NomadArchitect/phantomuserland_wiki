# Blocking system calls

See also: [[SnapSync]], [[ObjectKernelConnector]], [[KernelObjectInterface]], [[PhantomArchitecture]]

## Problem

Virtual machine code can not call long running or blocking kernel code. It will
break snapshots: it is impossible to save state of C code in kernel. On restart
only state of object land memory is restored, and VM threads continue to run from
last position.

## Solution

The ```opcode_sys``` instruction of VM is restartable.

Here is the way it works:

* VM program calls method of .internal class.
* Method code consists of one instruction - `sys <syscall number>`

Executing this instruction VM:

* Reads and stores method args from stack.
* Restores stack state as it was before `sys` instruction was executed.
* Moves VM IP back to the beginning of `sys` instruction
* Calls kernel code to execute required function

Now three scenarios are possible.

**Code implementing method does not block.**

It just returns to VM soon.

In other two cases kernel code revokes access to object memory by calling ```do_vm_unlock_persistent_memory()```, letting snapshots to happen.

**Kernel is running uninterrupted until kernel code is done and returned.** 

* Access to object memory is gained, preventing snapshots.
* Stack is cleared of `sys` arguments
* IP is moved forward to skip `sys` instruction
* Result object is pushed as a return value of `sys`
* Control is returned to VM interpreter, letting it to execute next instruction

**Kernel made a snapshot, was uninterrupted and reboot happened.**

* VM interpreter restarts thread and finds itself in front of a `sys` instruction, which caused all that.
* Execution of `sys` instruction is restarted from scratch.

## Rules

 * VM interpreter or JIT code must NOT call ```do_vm_unlock_persistent_memory()``` if state is NOT completely stored in memory. It can be called ONLY on the instructions border with all intermediate state is saved in persistent RAM. Not CPU registers, not local caches, not kernel. Assume that call to ```do_vm_unlock_persistent_memory()``` is like ```longjmp()``` to the entry point of interpreter.

 * If kernel accesses objects such objects must be added to restart list to be refdec'ed and killed on restart.
