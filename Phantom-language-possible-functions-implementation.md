# Functions (not implemented)

Class methods in Phantom Language are divided in two categories: functions and non-functions (actions?).


The difference is quite simple. Functions are always return value, but never modify any of arguments, including ‘this’.


Non-functions (actions?) can modify arguments and/or ‘this’ object, but can not return value.


Such a division makes Phantom a bit more functional-style language on the one side and is quite practical on the other: compiler can aggressively optimize expressions with functions – up to eliminating calls to them due to common subexpression merge. 


In addition this division clarifies code quite a bit. You can be sure that getting value from object you never modify it in any way.

