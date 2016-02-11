## Goal ##

Object land can not do a blocking call to kernel: this would prevent snapshot from being taken, because VM
state is interlocked with kernel state. In short, after restart there will be no one to return from that call.

Connection object is used to solve that. Connection is able to start kernel operation, block itself in the VM and wait for operation to finish. If restart happens, kernel calls special entry point of connection before VM start, letting connection to re-establish communications with kernel or return failure to VM side.

## Design ##

TODO write me

VM `.internal.connection` class implements this.

See `vm/connect.h` and `oldtree/kernel/phantom/vm_connect.c` plus `vm_cn_*.c` for examples.
