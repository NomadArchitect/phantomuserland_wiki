== Possible attributes implementation ==



Attributes are compile-time properties of variable, value or method/argument.


Some attributes are specifically treated by system, others are user-defined.



{| style="border-spacing:0;width:15.441cm;"
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| const
|| –
|| It is checked that corresponding value is constant. It will not be permitted to use such value in situations where it can be changed. Calls to const methods are subject to common sub-expression elimination.
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| volatile
|| –
|| Any kind of optimizations (for example, common sub-expression elimination) will be forbidden for this value/method.
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| synchronized
|| –
|| Each access will be locked on class-global semaphore.
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| class-local
|| –
|| can’t be passed/used outside of class (like private)
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| package-local
|| –
|| Compiler will generate error if you try to pass such thing to other package’s method somehow.
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| user-local
|| –
|| Compiler will generate code for runtime checks for passing this data to another user. (User is operating system defined thing.)
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| thread-local
|| –
|| Compiler will generate code for runtime checks (or do static checks where possible) for passing this data to another thread.
|- style="border:none;padding-top:0cm;padding-bottom:0cm;padding-left:0.191cm;padding-right:0.191cm;"
|| phantom-public
|| –
|| In phantom OS environment this class will be globally accessible. Other classes will be available in package only.
|-
|}


System attributes are always declared and are global. No attribute can use name of system attribute. User attributes are corresponding to some package.


Class can have attributes too. All methods and objects of that class will have corresponding attributes.


var name : <type-def-expression> attribute attr_name, const;


Defines variable with attributes “attr_name” and “const”. 


var name : <type-def-expression> @ attr_name, const;


Shortcut for “attribute” keyword is @.


var name : <type-def-expression> attribute attr_name!;




Exclamation sign marks attribute as required – it means that variable can be passed to method which has corresponding attribute mark.


var name : <type-def-expression> attribute ~attr_name;


This variable is explicitly not compatible with methods, marked with this attribute.


<method-definition> ::= 

<type-name> <method-name> '''('''<args>''')''' ['''['''<integer>''']'''] <attribute-list>

'''{''' <method-body> '''}'''


<args> ::= [ <arg> [, <args> ]]


<arg> ::= … <attribute-list>


<attribute-list> ::= [<attribute> [,<attribute-list>]]


<attribute> ::= '''attribute''' ['''~''']<identifier>['''!''']


Example:


string toString( string format attribute const ) attribute const;


<div style="margin-left:2.54cm;margin-right:0cm;">string toString( string format @ const ) @ const;</div>


Method attribute list is compared against ‘this’ attribute list. Method argument attribute list is compared against passed parameter or expression attribute list. Destination of assignment operator is compared against assigned expression attribute list.


Rules are:


{| 
|-
|  | 
|  | Passed value
|-
|  | Argument
|  | No attribute
|  | Has attribute
|  | Has attribute!
|  | Has !attribute
|-
|  | No attribute
|  | Ok
|  | Ok
|  | '''Forbidden'''
|  | Ok
|-
|  | Has attribute
|  | Ok
|  | Ok
|  | Ok
|  | '''Forbidden'''
|-
|  | Has attribute!
|  | '''Forbidden'''
|  | Ok
|  | Ok
|  | '''Forbidden'''
|-
|  | Has !attribute
|  | Ok
|  | '''Forbidden'''
|  | '''Forbidden'''
|  | Ok
|-
|}




In general, <u>~attribute</u> means “I don’t have this attribute and counterpart must not have it too”, <u>attribute!</u> means “I have this attribute and counterpart must have it too” and <u>attribute</u> means “I have this attribute but don’t require my counterpart to have it”.


Attribute can (must?) be defined:


attribute .const !-> ;

attribute .volatile + ->! ;

attribute .secure ! * ;

attribute .ru.dz.deprecated *;


Marking attribute with + means that result of any operation will have attribute if any of operands has it. Marking with * means that all operands must have attribute for result to have it too. If attribute is defined with !, all uses of it will define it as required (as if it was given with “!” mark). It is an error to mix attribute! and ~attribute operands in one expression. Attribute with !-> mark will be treated as ! in variable definitions, and ->! – as required in arguments.


By default attributes are multiplicative (‘*’ type) and non-requiring.


It is not possible to find at run time, has some value given attribute, or not.


Examples:


attribute .const !-> * ;


const int i = 10;

class { void index( int arg ); } object;

object.index( i ); // Error! i has required attribute


int j = 15;

class { void index( const int arg ); } cobject;

cobject.index( j ); // Ok


// Ok – because of * value of i+j is not const anymore.

object.index( i+j ); 


attribute .volatile !-> +;


volatile string vs;

string nvs, result;


// Error! Due to ‘+’ in attribute definition

// result of op is volatile, and requires destination

// to be volatile too because of ‘!->’.

result = nvs+vs; 


attribute .secure ->! *;


int a;

class { void use( secure int arg ); } sobject;

subject.use(a); // Error!








secure int a = (@secure) 10; // cast
