# Kernel to object land interface #

See also [[SnapSync]]

## Problems ##

* If kernel can access any persistent object, garbage collector should not delete such object.
* That's why reference count of object must be incremented as long as kernel can access it.
* And must be decremented if kernel looses reference to such object.
* Abrupt kernel stop (forced rebut or just loss of power) is loss of reference too and must be handled.

## Design ##

Any object kernel can access is acessed through handle, not sirectly by address, and allways added to so called restart list. Restart list is "kernel's reference" to object and this reference makes sure that object will not be gc'ed even if no other object references it.

On kernel boot restart list is rebuilt. Old one is moved off and new one initialized. Then old one is processed one item at a time - if item class is internal, special restart C function is called. After that reference is deleted (refcnt decremented), so if that is the last ref, object will die. Restart function can request putting object in new restart list, re-establishing kernel connection somehow, and in this case object will continue to exist.

Object can be removed from restart list manually, but serious care must be taken to ensure that no one will attempt to access it from kernel after that.

## API ##

```
# include <vm/root.h>

void pvm_add_object_to_restart_list( pvm_object_t o );
void pvm_remove_object_from_restart_list( pvm_object_t o );

# include <vm/khandle.h>

typedef int ko_handle_t;


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

## Usage example ##

```
ko_handle_t h;
errno_t rc;

rc = object_assign_handle( &h, o ); // got object from VM to kernel call, convert to handle

...

pvm_object_t o2;
rc = handle2object( &o2, h ); // get object address, increment refcount
o2.data->... // do something
rc = handle_release_object( h ); // stop using address, decrement refcount

...

rc = handle_release_object( h ); // finally stop using object at all, decrement refcount and remove from restart list


```
## Q ##

**Can restart list be compacted? Id reuse?**


# Details #

**pvm\_add\_object\_to\_restart\_list must return index (handle)** need f to map handle to ref
