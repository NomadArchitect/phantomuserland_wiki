
Type is either void, or class name reference with optional container modifier. Look at examples:

```
// error, variable can’t be void
var alpha : void; 

// scalar
var beta : .internal.integer; 
var beta : int; // '''int''' is shortcut for '''.internal.integer'''

// vector of strings, default implementation
var gamma : .internal.string [];
var gamma : string [];

// vector, own implementation (.local.my_vector is a class)
var gamma : .internal.string [.local.my_vector]; 

// vector of type, defined at runtime by class_variable.
var gamma : *class_variable [.local.my_vector]; 

// vector of strings, vector class is defined at runtime 
// by class_variable.
var gamma : .internal.string [*class_variable]; 
```


Type can be undefined at compile time.

```
// variable of unknown type
var alpha; 
```
