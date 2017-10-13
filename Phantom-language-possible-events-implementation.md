# Events (not implemented)

Class can have special event fields.

```
class car 
{
  event crash;
  void do_crash { crash++; }

} mycar;

…

mycar.crash += { print(“My car is dead now!\n”); }
```

Event handlers (closures, assigned to events) can have one parameter, which will receive signaling object reference:

```
class car 
{
    event crash;
    void do_crash { crash++; }
} mycar;

…

mycar.crash += 
    (car src) 
    { 
        print(“My car (“+src.toString()+”) is dead now!\n”); 
    }
```

Or:

```
class garage
{
    handle_crash(car src);
} my_garage;

…

mycar.crash += my_garage.handle_crash;
```

