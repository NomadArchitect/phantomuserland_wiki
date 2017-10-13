# Errors and plans

Do <class-name> and <type-name> mean the same in this document? (No, type is class with possible []s)


Constructor can have ‘regenerator’ semantics to let us automatically recreate object if it was lost intentionally (not saved on snap) or accidentally (disk error, network error).


Make-style object dependencies?


Automatic-like variables with a guaranteed finalizer call on out-of scope event? (It will be forbidden for them to be passed away from method, of course.)


When passing object reference losing its type, we shall replace its interface with a special one, which will check all argument types in runtime.


Shall class object have a method which returns class interface by index?


case ranges? case 1 … 5:


## Closures (not implemented)

Closure is a pair of object pointer and block of code. Closure can be passed as value and activated. Closure can have parameters (in this case we can call it anonymous method). Closure can use usual method as it’s code.


Need closure to get parameters from surrounding code? Implicit or explicit way?


