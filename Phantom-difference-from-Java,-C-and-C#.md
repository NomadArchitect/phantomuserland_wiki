# Phantom language differences from Java, C# and C++

For those who don't want to read too much. 

## No difference between integer and object. 

Everything is object.

## Operator == compares value (calls .equals()). To compare references use :== and :!=

```
x = map.get(“apple”);
if( x :== null ) ...
if( x == “macintosh” ) ...
```

## dynamically typed new operator 

Accepts not only type name, but also a class object to create instance of. Such as 

`x = new *(expression-giving-class-object)(c'tor params);`

## Var definitions are Pascalish. 

Sorry about that, will return to c/java style soon.

`var i : int;var container : .phantom.util.map;`

## Operator .= is value assignment. 

Not really implemented.

## Type names are either absolute (start with '.') or relevant to package. 

Possibly will change and relevant names will be searched in imported packages as well.



