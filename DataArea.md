# Internal object data area #

Internal class is like Java's native code. It looks like usual class from outside, but implemented
inside of Phantom's kernel.

Objects of internal class can contain any variables C language can handle. That's what we call DataArea.

See include/vm/internal_da.h for details.
