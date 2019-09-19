# VM Class loader and linker

See also [[ClassFile]]

## Basic class load

Class is represented as binary blob containing tagged records:

### 'C' - class record

Defines class name, base class name, number of object fields and methods.

### 'M' - Method

### 'l' - IP to line number map

### 'S' - method signature

String signature, int32 ordinal, int32 number of args, int32 is constructor.

### 'c' - constant for const pool

int32 index in const pool, type, value. Currently just String type is supported.

TODO: Add loader flags to process value later, see below.

### 'f' - field names

List of name/ordinal parts for fields.

TODO: add types?!

## (not implemented) resolver part

Constant record can have marker that requests it to be
postprocessed after load:

* ConstIsClassRef - string must be resolved to class object and replaced with reference.
* ConstIsMethodOrdinal - string must be resolved to class method and replaced with method ordinal (int).
* ConstIsFieldOrdinal - string must be resolved to class field and replaced with method ordinal (int).

Ordinal references includes class name, which has to be passed separately, so, possibly, we need 
this flag to be the first field of record to know how to load the rest.
