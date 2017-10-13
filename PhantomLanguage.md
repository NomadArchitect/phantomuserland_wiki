Phantom programming laguage is a native language for Phantom applications code. There is also (incomplete) convertor from JVM to Phantom VM which permits use of JVM based languages in Phantom.

# Phantom language reference


This document describes phantom programming language as planned at this moment, 10 March 2009.

Impatient? Read [[Phantom difference from Java, C and C#]]


## Structure

Phantom language is a quite classic object-style programming language. 

### Objects, types

Everything is an object. Type (class) is object that is able to create objects. Class class is a root thing used to create classes.

### Built in, atomic types

At this moment phantom VM is able to process objects of some predefined classes – integer, long, float, double, string, code, interface, class. All of them look like usual classes/objects, no special treatment from outside is needed. To be more effective VM is able to process integers separately, in a more effective way. Such integers are specific only during processing and stored as usual objects too.

### Type checks

Phantom is a dynamically typed engine. Any object’s type can be (but is not) checked at runtime. Static type checks are done where possible, but in general it is possible to ignore types completely at compile time.

### Exceptions

Any class can identify an exception (no special interface implementation is required, for example, for object to be thrown). Any object can be thrown, and catcher will catch it if object belongs to the type that catcher is looking for…

### Calls, returns

Any number of arguments is passed, one or nothing is returned. Null object will be received if caller expects return value and called code did not return anything. (This is assumed to be impossible in a correct Phantom language program, but is possible technically.) If called code does not return some object explicitly, it is possible that some junk value will be returned. Static type checks are supposed to prevent this, but you can overcome.




To do: named arguments, such as
```
container.add( value => 5, position => iterator );
```
## Memory

Phantom language objects are dynamically (heap) allocated only. No automatic (stack) allocation possible, but stacks variables exist. No static variables exist, but you can use class variables to achieve effect akin to static variables (not implemented).


Phantom code is expected to be running in a persistent environment, but can be used in conventional environment too.

## Flow control

### if
```
if( <integer-expression> ) <operator-yes> [else <operator-no>]
```

<u>Operator-yes</u> will be executed if <u>integer-expression</u> is non zero. If <u>operator-no</u> is given it will be executed if <u>integer-expression</u> is zero. If type of expression can not be checked at compile time, '''NO CHECKS''' will be done at runtime. Result is unpredictable. Don’t expect exception to be thrown.


### while
```
while( <integer-expression> ) <operator>
```

As long as <u>integer-expression</u> is nonzero, <u>operator</u> will be executed in loop. If type of expression can not be checked at compile time, '''NO CHECKS''' will be done at runtime. Result is unpredictable. Don’t expect exception to be thrown.


### do-while
```
do <pre-operator> while( <integer-expression> ) [<post-operator>]
```

Same as while, but <u>pre-operator</u> is executed before any condition check. If <u>post-operator</u> is omitted, we have a classical C-style do-while operator.


### foreach

Not impl?

```
foreach( <variable> in <container-expression> ) <operator>
```

<u>Operator</u> will be executed a number of times, and each time <u>variable</u> will be assigned a new element of <u>container-expression</u>. All elements will be iterated through. Order is defined by the container. Variable will be const.

Todo: “foreach(iterator) operator”

### switch

```
switch( <expression> )
{
    case <expression>:
        // code
        break;

    default:
        // if expression was 1 we’ll come here
        break;
}
```


Your plain vanilla switch operator. Difference is that you can use any data type and non-constant cases. Integer and constants are MUCH more effective, though.



### return

```
return;

return <expression>;
```

Return from method, with or without value.



### try-catch
```
try 
    <operator> 
catch( <type-1> <variable-1> ) 
    <operator-1>
...
```

If <u>operator</u> will raise an exception throwing value of such <u>type-1</u>, <u>operator-1</u> will be executed, with <u>variable-1</u> containing thrown value.


### throw

throw <expression>;


Exception will be thrown and control will be passed to catching code.


### assert 

Not impl?

assert <expression> [throw <value-to-throw>];

assert [typeof] <expression> is <type-expression> [throw <value-to-throw>];


In a first form, throw will be executed if <u>expression</u> is zero. Second form checks for <u>expression</u> to be of a given type.

### new

```
… = new <type> ( [<constructor-parameters>] );

… = new *(<type-expression>) ( [<constructor-parameters>] );
```

In a first form, creates an object of a statically given type. Second form uses <u>type-expression</u> as class object. If expression type can not be checked at runtime, you have a chance of getting runtime exception (of object is not a class or args are of incorrect type).

## Variables

All and every variable and programmer-accessible object field in phantom is a reference. Integer in Phantom IS, unlike Java, an object.


Typeless variable can be declared like this:

```
var name;
var name[];
```

Statically typed variable is:

```
var name : <type_name>; // scalar variable
var name : <type_name>[]; // container
```

or:

```
<type_name> name; // scalar variable
<type_name>[] name; // container
```

(The last form is not implemented yet.)


Dynamically typed variable:

```
var name : *<type-defining-expression>;
var name : *<type-defining-expression>[];
```

Expression must evaluate to class (type) object. It will be checked at compile time if possible.











## Expressions

### Assignment 

<lvalue> '''<nowiki>=</nowiki>''' <expression>'''<nowiki>;</nowiki>'''


Usual = is a reference assignment operator.

```
var i1 = 5; // new integer object is created
var i2 = i1; // i2 is the same object as i1 now
i1 = 10; // i2 is 5 now! Not like Java!
```

Note that '''<nowiki>=</nowiki>''' operator works more like in C/C++, though it works with references, not values.


<lvalue> .'''<nowiki>=</nowiki>''' <expression>'''<nowiki>;</nowiki>'''


Value assignment. This is a shortcut for a method ‘assign’ (ordinal '''N - 11??''') of an object. Thing to kill if you like functional languages. Do we need to forbid it for integer and string objects?


- implemented, -tested.

=== Comparison ===

<lvalue> '''<nowiki>=</nowiki>''' <expression> ''':==''' <expression>'''<nowiki>;</nowiki>'''

<lvalue> '''<nowiki>=</nowiki>''' <expression> ''':!=''' <expression>'''<nowiki>;</nowiki>'''




Operator ''':==''' is reference comparison.

```
var i1 = 5; 
var i2 = i1; 

i1 = 5; 
i1 :== i2 // is false!
```

Note that :'''<nowiki>==</nowiki>''' operator works more like == in Java.


<expression> '''<nowiki>==</nowiki>''' <expression>

<expression> '''!=''' <expression>

<expression> >'''<nowiki>=</nowiki>''' <expression>

<expression> <'''<nowiki>=</nowiki>''' <expression>

<expression> > <expression>

<expression> < <expression>




Value comparison. This is a shortcut for a method ‘equals’ (ordinal '''N??''') of an any type object and internal operations for int and string. 


### Import

String (binary array, in fact, but currently compiler treats it as a string) can be imported from some storage. This is a quick hack to make an easy way to bring binary data to phantom testing environment. Import will surely change or cease to exist later. But for now compiler accepts constructs of '''import “filename”''' form in place of string constant.


string wavdata;

wavdata = import “wink.wav”;




## Method call 

### Normal call 

<lvalue> '''<nowiki>=</nowiki>''' <expression>.<method-name>'''('''<args>''');'''

### Call by ordinal 

<lvalue> '''<nowiki>=</nowiki>''' <expression>.<integer-constant-expression>'''('''<args>''');'''


All the methods internally are numbered. For very special system-programmer’s needs we have this non-recommended and non-safe way of calling things. Since method prototype is not known to compiler, no argument type checks are done. (Is it right? Usually we can find prototype by ordinal too…) Over more, most of things called this way will not check their arguments so if you pass them wrong, the exception you will get is not guaranteed to be descriptive enough.


## Class definition 



'''class''' <class-name> {<extends-implements>}

'''{'''

{<variable-or-method-definition>}

'''};'''


<extends-implements> ::=

<extends> | <implements>


<extends> ::= extends <class-name>


<variable-or-method-definition> ::=

<variable-definition> |

<method-definition>


<method-definition> ::= 

<type-name> <method-name> '''('''<args>''')''' ['''['''<integer>''']''']

'''{''' <method-body> '''}'''


Construction [<integer>] after arguments is used to define which ordinal number precisely this method will get.




'''package''' <package-name> '''<nowiki>;</nowiki>'''


Defines current source file as a component of a named package. It is not required. If package is not defined, all class definitions must use absolute class names (starting with point).

```
package .ru.dz.test;
class test { … };
```

Or:

```
class .ru.dz.test.test { … };
```

## Inheritance and interfaces (partially implemented)

Class can implement any number of interfaces:

```
class car implements .ru.dz.cars.general, .com.microsoft.docsource
{
…
};
```

Class can statically (in a quite effective way) inherit from one base class:

```
class car_truck extends car
{
…
};
```


## Types

[[Phantom language types]]



## Precedence

```
. [] ()

<nowiki>= .=</nowiki>

|| && !

<nowiki>== != < > <= >=</nowiki>

:== :!=

+ -

<nowiki>* / %</nowiki>

| & ~
```



## References 

[[InternalClasses]]

## See also

[[Phantom language possible attributes implementation]]
[[Phantom language possible interface cast implementation]]
[[Phantom language possible events implementation]]
[[Phantom language possible functions implementation]]

[[Phantom language assorted ideas]]
