# Phantom compiler and byte code generator

See also [[PhantomLanguage]]

Source code is in **tools/plc**

Phantom language compiler is used as compiler itself and as backend (byte code generator) for other frontends, namely - jvm to phantom convertor (jpc).

## Debugging

1. See **/test/plc** directory. This test can be used if you need to check if compiler generates same code as before. Just run tests and compiler listing files will be compared with reference ones. Note that it runs compiler defined in build/bin & build/jar. Usually it is not the latest code you made in tools/plc.
2. phatom/vm/pvm_test can be run to execute compiled bytecode. See also [[Debugging]].
3. Unit tests in tools/plc/src/test

## Class reference - compiler

### Grammar

Parser itself, recursive one. Most methods parse one source code construct, such as expression or operator. returns Node, which is AST tree node. Uses Lex as lexical analyzer, ParserState to keep track ow what is being comiled (class, method, maps of stack variables) and list of referenced classes.

## Internal representation

### Node

Node class represents node of abstract representation of program.

```find_out_my_type()``` - if possible, decide about this node's expression type by looking at descendants (parameters) or by node nature.

```preprocess()``` - must be called after we finished all parsing and before we're trying to do something else, such as generatimg code or just printing/listing.

```generate_code()``` - lays out bytecode for this subtree.

```generateLlvmCode()``` - llvm code generator, incomplete

## Implementation notes

### Untyped vars

Historically it was possible to define variable with no type. It is still possible for fields, method parameters,
but is deprecated.


## See also

* [[Phantom difference from Java, C and C#]]
* [[PhantomLanguage]]
* [[Phantom language types]]
* [[PhantomTestEnvironment]]

## Design notes

* [[Phantom language assorted ideas]]
* [[Phantom language possible functions implementation]]
* [[Phantom language possible events implementation]]
* [[Phantom language possible attributes implementation]]
* [[Phantom language possible interface cast implementation]]
* [[ObjectOrdinals]]
* [[Satellites]]
* [[VirtualMachine]]
* [[VmClassLookup]]
* [[VmLinker]]
