## Nearest Goal ##

To produce distributable alpha-level OS which can be programmed for by 3rd party programmers.


## Userland/Tools Tasks ##

Quite crirtical tasks.

|Topic|State|
|:------------------------------------------------------------------------------------------------------------------------------|:-----------------|
| **Java bytecode compiler**. Currently can compile very simple code. Need complete compiler and classpath. At least to the level where simple Web server can be implemented. See [Java-Compiler-0.5 milestone](https://github.com/dzavalishin/phantomuserland/milestones/Java-Compiler-0.5)| **Hi priority.** |
|**Hosted test env. multithreading**. Port existing threads lib to work in unix userland/cygwin.|**Debugging**|
|**Hosted test env. TCP access**.|In progress.|
|Windowing events, input, event thread pools. Need userland iface. Need code cleanup.|**In progress**|
|**VM and compiler support for floats/doubles/64 bit ints**.|**Mostly done**|
|**More of Phantom language implementation in compiler**. Closures, direct containers support.|Actual?|
|**VmWeakRef** - stuck with synchronization!|**Deferred**|
|**Border objects and restart exceptions**. Especially for TCP interfaces. Depends on weakrefs!|**Nearly done**|


## Kernel Tasks ##

|Topic|State|
|:------------------------------------------------------------------------------------------------------------------------------|:-----------------|
|**VirtIO drivers**. Have working disk driver, net driver in progress. Kernel hangs if using virtio driver for paging. #181 |Finish it!|
|**IDE driver cleanup**. Existing driver is very strange.|**started**|
|**IP stack tests and userland interface**.|- |
|**ARM port**. Machdep code and some drivers are done.|In progress|
|**Partition lookup code**. |PC partitions implemented. EFI?|
|**Disk selection**. Right now Phantom looks for its disk in hardcoded places. Partitioning system recognizes Phantom partitions and disks. Has to be connected to pager.|Done.|

## Far Goals ##

|Topic|State|
|:------------------------------------------------------------------------------------------------------------------------------|:-----------------|
|**Graphical shell**. Something to start with.|Nope.|
|**Posix support**. Map two binary objects to DS/CS, run native thread, support syscall mapping.|**Partially working.**|
|**LLVM fast code**. In addition to posix subsystem it would be nice to have a specific LLVM subsystem which can offer programmers hispeed, but portable (to Phantom on other arch) programming framework. The same CS/DS binary objects, but executable code is LLVM-level and passed through LLVM backend before being run for the first time. See [LlvmCodegen.java](https://github.com/dzavalishin/phantomuserland/blob/master/tools/plc/src/ru/dz/plc/compiler/LlvmCodegen.java)|Sompe tests in plc.|
|**OpenGL**. In fact, it is quite easy to get TinyGL working (it is already done for kernel), but we need a clean userland interface and understanding of interoperation for multiiple simult. threads doing GL rendering in a same env.|Just an userland lib?|
|**Sound drivers**. At least some very simple AC97 wave output one?|**In progress**|
|**Video codecs environment**. Seems to be requiring Posix/LLVM support first. The first goal is to be able to give it an MPEG binary for playing.|**Quite near now**|


# General Project Support Tasks #

  * **Doxygen**. And code documentation too.
  * **Test suite**, regular regression test setup. **mostly ok**
  * **Code review**. Need code review process to be set up and carried.
  * **Good internal document(s) on development process**. For new members to jump in easily. Here is it: <https://phantomdox.readthedocs.io/en/latest/>
  * **Define branching/merging rules**. We will have to have release branches.


# Later... #

  * **Python compiler**. Seems to be a good possible object env shell and common scripting language. Or shall it be Ruby instead? Or maybe do something strange and do own scripting language (Maybe better because direct control over how it works and better optimization)? 

# More... #

  * Phantom over Genode <https://github.com/S7rizh/phantomuserland>
  * Phantom/Genode build env <https://github.com/dzavalishin/phantom_build_env>

