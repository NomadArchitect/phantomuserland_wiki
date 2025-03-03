# Introduction #

See also: [[InternalClasses]]

Syscall is a method of internal class. It is called as usual method (at least all the conversion is hidden). 
Syscall is supposed to return one object or return nothing. Otherwise it can throw an exception.
Syscall is supposed to decrement refcount for its object arguments or save/pass them along.

Every syscall is implemented wit a C function.

C function parameters: 
* pvm_object_t me - 'this' object
* pvm_object_t *ret - place for an object to return
* struct data_area_4_thread *tc - current thread object [[DataArea]] pointer
* int n_args - number of args passed
* pvm_object_t *args - args

C function implementing syscall must return 1 on success or 0 to throw an exception. In this case *ret parameter
is not a syscall return value, it is an object we throw.

## Helpers ##

  * SYSCALL\_RETURN(pvm_object_t obj) - return object
  * SYSCALL\_RETURN\_STRING(const char *str) - return string
  * SYSCALL\_RETURN\_INT(int retval) - return integer
  * SYSCALL\_RETURN\_NOTHING - just return (null object will be returned, in fact)
  * SYSCALL\_THROW(pvm_object_t obj) - return by throwing an exception
  * SYSCALL\_THROW\_STRING(const char *str) - shortcut, accepts C string as arg
  * AS\_INT(pvm_object_t obj) - converts object it to C integer, with type check
  * CHECK\_PARAM\_COUNT(int must\_have) - throws a message if parameter count is wrong
  * ASSERT\_STRING(pvm_object_t obj) - throws, if not a string object
  * ASSERT\_INT(pvm_object_t obj) - throws, if not an int object
  * DEBUG\_INFO - in debug mode prints call info

## Example ##

```c
static int si_string_8_substring( pvm_object_t me, pvm_object_t *ret, struct data_area_4_thread *tc, int n_args, pvm_object_t *args )
{
    DEBUG_INFO;
    ASSERT_STRING(me);
    struct data_area_4_string *meda = pvm_object_da( me, string );
    
    CHECK_PARAM_COUNT(2);

    int parmlen = AS_INT(args[1]);
    int index = AS_INT(args[0]);

    if( index < 0 || index >= meda->length )
        SYSCALL_THROW_STRING( "string.substring index is out of bounds" );

    int len = meda->length - index;
    if( parmlen < len ) len = parmlen;

    if( len < 0 )
        SYSCALL_THROW_STRING( "string.substring length is negative" );

    SYSCALL_RETURN(pvm_create_string_object_binary( (char *)meda->data + index, len ));
}
```

## Blocking code ##

Code implementing syscall must finish its job really quick or it will prevent snapshots from being done. If you need
long waiting code in syscall, call ```vm_unlock_persistent_memory()``` before it and ```vm_lock_persistent_memory()``` after.

Note that you can't snd shouldn't access persistent storage (any VM objects) in this state. You can briefly
re-gain access to persistent memory by calling ```vm_lock_persistent_memory()``` again (and not forget to ```vm_unlock_persistent_memory()``` soon), but be cautious.

Example:
```
vm_object_t array = pvm_create_object( pvm_get_array_class() );
vm_unlock_persistent_memory();
for(...)
{
    const char *string = call_network();

    vm_unlock_persistent_memory();
    pvm_object_t so = pvm_create_string_object(string);
    pvm_append_array( array, so );
    vm_unlock_persistent_memory();
}

vm_lock_persistent_memory();
```

## Checklist to create internal class

sys/i_CLASSNAME.{c,h}

internal.c - descriptor

vm/internal.h - DEF_I(name)

root.h - add CLASSNAME_class to pvm_root_t, #define PVM_ROOT_OBJECT_???_CLASS <next num>

root.c 

 - set_root_from_table():     SET_ROOT_CLASS(classname,CLASSNAME);
 - define pvm_get_XXX_class()

**Write documentation!**

## Outdated, do not use ##

  * SYSCALL\_PUT\_THIS\_THREAD\_ASLEEP() - after returning from syscall current thread will be blocked
  * SYSCALL\_PUT\_THREAD\_ASLEEP(thread) - thread will be put asleep AFTER SYSCALL ONLY! In fact this one can't be used!
  * SYSCALL\_WAKE\_THIS\_THREAD\_UP() - impossible to call :)
  * SYSCALL\_WAKE\_THREAD\_UP(thread) - given thread will be unblocked NOW.

