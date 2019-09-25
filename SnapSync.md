# Goal

* To prevent persistent memory from being modified during snapshot.
* To make sure virtual machine state is consistent during snapshot.

See also [[ObjectKernelConnector]]

# Functions

## Guard access to persistent memory

If yor code accesses persistent memory sporadically or unfrequently:

`void vm_lock_persistent_memory( void )`

Enable access to persistent memory for current thread. Blocks snapshots from being started.
Can block on call if snapshot is requested or in progress.

`void vm_unlock_persistent_memory( void )`

Revoke persistent memory access rights for current thread. If thread attempts to access
object space after calling this function, kernel will panic.

If your code works with persistent memory frequently, assume following style:

```
vm_lock_persistent_memory();
while(1)
{
    if(phantom_virtual_machine_snap_request)
        phantom_thread_wait_4_snap();
    ... your code ...
    // NB! No cond wait or long locking mutexes in loop!
}
```

**NB!**

This example is good just for **TIGHT** loops with no wait inside the 'while' statement.
Even sleep for more than few milliseconds is not ok. To perform a critical part of snapshot kernel
waits for all threads to revoke object land access, and if it takes too long it may become
visible for user.

`extern int phantom_virtual_machine_snap_request;`

This variable becomes non-zero if Phantom is going to start next snapshot.

`void phantom_thread_wait_4_snap( void );`

This function releases access to persistent memory, sleeps until snapshot is done
(usually it takes milliseconds), requests access to persistent memory back and returns.


