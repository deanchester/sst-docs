---
title: SST_ELI_REGISTER_PORTMODULE
sidebar_label: PortModule
---

```cpp
SST_ELI_REGISTER_PORTMODULE(class_name, "library", "name", 
    SST_ELI_ELEMENT_VERSION(major, minorX, minorY), "description")
```

All PortModules must register themselves with SST using this macro. The library and name strings provided in this macro will be used by SST to identify the port module as "library.name". The version and description are displayed by `sst-info` to document the version and purpose of the module.

:::info Important
This macro must reside in a `public` section of the PortModule's header file.
:::


## Parameters

* **class_name** (class) The name of the PortModule class. This is not a string.
* **library** (string) The name of the library that this PortModule belongs to. If the library name does not exist, it will be created.
* **name** (string) The name that will be used to instantiate this PortModule in the simulation input configuration. It can be the same as the class_name but does not need to be. The full name of the PortModule will be `library.name`.
* **SST_ELI_ELEMENT_VERSION(major, minorX, minorY)** This is a macro that specifies the version of a PortModule. `major`, `minorX`, and `minorY` are integers that form a version number major.minorX.minorY. For example: SST_ELI_ELEMENT_VERSION(3, 0, 9) yields a version of 3.0.9. Versions are not checked by SST; this is provided for developers to version and manage their libraries.
* **description** (string) A description of the port module

:::info
`library` and `name` must follow SST's [element naming conventions](../../../guides/dev/naming.md).
:::

## Example

### Registering a PortModule
The following example is taken from SST's built-in 'RandomDrop' port module which randomly drops events. It is part of the 'sst' element library. The fully qualified name of the module is 'sst.portmodules.random_drop'.

```cpp
#include <sst/core/portModule.h>

namespace SST {
class RandomDrop : public SST::PortModule
{
public:

    //highlight-start
    SST_ELI_REGISTER_PORTMODULE(
        RandomDrop,                         // PortModule class
        "sst",                              // Library name, the 'lib' in SST's lib.name format
        "portmodules.random_drop",          // Name used to refer to this port module, the 'name' in SST's lib.name format
        SST_ELI_ELEMENT_VERSION(0, 1, 0),   // A version number
        "Randomly drops events"             // Description
    )
    //highlight-end

    /* Rest of class here */
};

} /* End namespace SST */
```


## Header
```cpp
#include <sst/core/portModule.h>
```
