## Object Headers ##

There are two object headers. Allocator header and VM header.

### Allocator header ###

```
struct object_PVM_ALLOC_Header
{
    unsigned int                object_start_marker;
    volatile int32_t            refCount; // for fast dealloc of locally-owned objects. If grows to INT_MAX it will be fixed at that value and ignored further. Such objects will be GC'ed in usual way
    unsigned char               alloc_flags;
    unsigned char               gc_flags;
    unsigned int                exact_size; // full object size including this header
};
```

 * object_start_marker - just magic to make sure (well, reduce uncertaincy:) it is an object.
 * refCount - used by fast refcount GC. To be removed and replaced by generation based GC.
 * alloc_flags - used by allocator code
 * gc_flags - used by GC code
 * exact_size - size of allocated area


### VM Header ###

```
struct pvm_object_storage
{
    struct object_PVM_ALLOC_Header      _ah;

    struct pvm_object                   _class;
    struct pvm_object                   _satellites; // Points to chain of objects related to this one
    u_int32_t                           _flags; 
    unsigned int                        _da_size; // in bytes!

    unsigned char                       da[];
};
```

 *  _ah is allocation header, used by allocator/gc
 *  _class is class object reference.
 *  _satellites is used to keep some related things such as weak ptr backlink, see [[Satellites]]
 *  _flags used to keep some shortcut info about object type
 *  _da_size is n of bytes in da[]
 *  da[] is object contents


