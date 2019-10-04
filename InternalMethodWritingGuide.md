# Introduction #

See also: [[InternalClasses]]

Syscall is a method of internal class. It is called as usual method (at least all the conversion is hidden).

Syscall is supposed to return one object or return nothing. Syscall can throw an exception.

Syscall is supposed to decrement refcount for its object arguments or save/pass them along.

Q: what about 'this'?

Call parameters: 'this' object, current thread object [[DataArea]] pointer.

# Helpers #

  * SYSCALL\_RETURN(obj) - return object
  * SYSCALL\_RETURN\_NOTHING - just return (null object will be returned, in fact)
  * SYSCALL\_THROW(obj) - return by throwing an exception
  * SYSCALL\_THROW\_STRING(str) - shortcut, accepts C string as arg
  * AS\_INT() - converts object it to C integer, with type check
  * CHECK\_PARAM\_COUNT(must\_have) - throws a message if parameter count is wrong
  * ASSERT\_STRING(obj) - throws, if not a string object
  * ASSERT\_INT(obj) - throws, if not an int object
  * DEBUG\_INFO - in debug mode prints call info

# Example #

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

# Outdated, do not use #

  * SYSCALL\_PUT\_THIS\_THREAD\_ASLEEP() - after returning from syscall current thread will be blocked
  * SYSCALL\_PUT\_THREAD\_ASLEEP(thread) - thread will be put asleep AFTER SYSCALL ONLY! In fact this one can't be used!
  * SYSCALL\_WAKE\_THIS\_THREAD\_UP() - impossible to call :)
  * SYSCALL\_WAKE\_THREAD\_UP(thread) - given thread will be unblocked NOW.

