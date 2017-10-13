
One can reduce object method reference’s method list by casting it to some interface of object’s class. Like:

```
interface .local.my.redonly 
{ 
    string get_value(); 
};

class .local.my.readwrite implements .local.my.redonly

{ 
    string get_value(); 
    void set_value(string); 
};


var stuff = new .local.my.readwrite;

stuff.set_value(“hello, silent world”);

return (.local.my.redonly)stuff;
```


Now caller can not access set_value method. Returned object’s interface just has no reference to it. And cast to .local.my.readwrite will not return that reference back.


**NB!** It will be good to have `return (@const) stuff;` to strip all non-const methods, by the way.


