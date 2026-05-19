---
title: Developing FAQ
---

## Building Elements
### Do I have to use SST's Autotools build system for my elements?
No, you may use the build system of your choice for you own element libraries. SST is regularly tested with element libraries built using Autotools, CMake, and raw Makefiles.

### My element library compiled and I registered it, but SST can't load it. 
This typically occurs because SST was unable to link to your library, likely because of missing symbols. By far the most common cause of this issue is that a class has a missing function definition, with destructors and default constructors being especially common culprits. Check for functions that are declared but not defined.


