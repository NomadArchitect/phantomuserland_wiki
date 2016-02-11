## Design ##

On kernel boot restart list is rebuilt. Old one is moved off and new one initialized. Then old one is processed one item at a time - if item class is internal, special restart C function is called. After that reference is deleted (refcmt decremented), so if that is the last ref, object will die. Restart function can request putting object in new restart list, re-establishing kernel connection somehow, and in this case object will continue to exist.

Object can be removed from restart list manually, but serious care must be taken to ensure that no one will attempt to access it from kernel after that.

## API ##

```
# include <vm/root.h>

void pvm_add_object_to_restart_list( pvm_object_t o );
void pvm_remove_object_from_restart_list( pvm_object_t o );

# include <vm/khandle.h>

typedef int ko_handle_t;

errno_t  object_assign_handle( ko_handle_t *h, pvm_object_t o );
errno_t  object_revoke_handle( ko_handle_t h, pvm_object_t o );


// These two work after handle is assigned
errno_t  object2handle( ko_handle_t *h, pvm_object_t o );
errno_t  handle2object( pvm_object_t *o, ko_handle_t h );

// Dec in-kernel refcount incremented by handle2object
errno_t  handle_release_object( ko_handle_t *h );


errno_t ko_make_int( ko_handle_t *ret, int i );
errno_t ko_make_string( ko_handle_t *ret, const char *s );
errno_t ko_make_string_binary( ko_handle_t *ret, const char *s, int len );

errno_t ko_make_object( ko_handle_t *ret, ko_handle_t ko_class, void *init ); // C'tor args? Or call it manually?

errno_t ko_get_class( ko_handle_t *ret, const char *class_name );

//! Get method ordinal by name - BUG - what if class is reloaded? Or we are tied to class version and pretty sure?
errno_t ko_map_method( int *ret, ko_handle_t *ko_this, const char *method_name );

//! Sync call - returns value - no, will hang snaps!
//ko_handle_t ko_call_method( ko_handle_t *ko_this, int method, int nargs, ko_handle_t *args, int flags );

//! Async call - void, does not wait
void ko_spawn_method( ko_handle_t *ko_this, int method, int nargs, ko_handle_t *args, int flags );

//! Async call - void, does not wait, calls calback
void ko_spawn_method_callback( ko_handle_t *ko_this, int method, int nargs, ko_handle_t *args, int flags, void (*callback)( void *cb_arg, ko_handle_t *ret ), void *cb_arg );


```

## Q ##

**Can restart list be compacted? Id reuse?**


# Details #

**pvm\_add\_object\_to\_restart\_list must return index (handle)** need f to map handle to ref
