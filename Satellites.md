## Introduction ##

Sometimes it is needed to attach a technical, worker object to some other object. For example:

 * When object has a weak ref on it, we need pointer to a weak ref object from object weak ref points to, so that GC can clear weak ref on object deletion. (See [[VmWeakRef]])
 * If object is used by Java-style sync primitives
 * Container object to hold data invisibly attached by some user/class
 * Owner or rights
