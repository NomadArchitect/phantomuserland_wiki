## functions ##

* kvprintf - general kernel print formatter, needs putc style output func
* lprintf - prints directly and only to debug console (serial 0 on PC and most builds, goes to QEMU log file too), slow, can work in closed interrupts and interrupt handlers, see `$ARCH/debug_console.c`
* SHOW_ERROR/SHOW_FLOW/SHOW_INFO - see below, main kernel logging, goes to debug window and debug console


## kvprintf additional formats ##

### %b ###

The format %b is supported to decode error registers.

Its usage is:

`	printf("reg=%b\n", regval, "<base><arg>*");`

Where <base> is the output base expressed as a control character, e.g.
\10 gives octal; \20 gives hex.  Each arg is a sequence of characters,
the first of which gives the bit number to be inspected (origin 1), and
the next characters (up to a control character, i.e. a character <= 32),
give the name of the register.  Thus:

`	kvprintf("reg=%b\n", 3, "\10\2BITTWO\1BITONE\n");`

would produce output:

`	reg=3<BITTWO,BITONE>`

### %D ###

Hexdump, takes pointer and separator string:
```
		("%6D", ptr, ":")   -> XX:XX:XX:XX:XX:XX
		("%*D", len, ptr, " " -> XX XX XX XX ...
```
## debug_ext.h ##

```
#define DEBUG_MSG_PREFIX "port"
#include <debug_ext.h>
#define debug_level_flow 10
#define debug_level_error 10
#define debug_level_info 10
```

```
SHOW_ERROR0( 0, "Failed" ); // No printf args
SHOW_ERROR( 1, "Hash remove fail for %s", el->name ); // Has args, will print if debug_level_error >= 1
SHOW_FLOW0( 0, "port handle init"); // Flow control debug prints
SHOW_INFO( 0, "irq = %d", irq ); // Regular logging
```


