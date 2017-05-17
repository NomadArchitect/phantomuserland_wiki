Toolchain for each architecture is defined in ```phantom/Makeconf-ARCH```, such as

* Makeconf-ia32 for 32bit Intel
* Makeconf-e2k for Elbrus 2000

Usually you need to define

* ARCH_FLAGS - specific compiler flags
* BIN_PREFIX - prefix for platform binaries, for example e2k-linux-gnu- for e2k cross-compilation

You can add local (compile site) (re)definitions in local-config.$(ARCH).mk in the root of Phantom directory tree
