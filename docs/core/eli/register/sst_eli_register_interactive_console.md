---
title: SST_ELI_REGISTER_INTERACTIVE_CONSOLE
sidebar_label: Console
---

```cpp
SST_ELI_REGISTER_INTERACTIVE_CONSOLE(class_name, "library", "name", 
    SST_ELI_ELEMENT_VERSION(major, minorX, minorY), "description")
```

InteractiveConsoles must register themselves with SST using this macro. The library and name strings provided in this macro will be used by SST to identify the console as "library.name". The version and description fields document the version and purpose of the console. 

:::info Important
This macro must reside in a `public` section of the InteractiveConsole's header file.
:::

:::info
`library` and `name` must follow SST's [element naming conventions](../../../guides/dev/naming.md).
:::

## Parameters

* **class_name** (class) The name of the InteractiveConsole class. This is not a string.
* **library** (string) The name of the library that this console belongs to. If the library name does not exist, it will be created.
* **name** (string) The name that will be used to request this console on the command line or in the simulation input configuration. It can be the same as the class_name but does not need to be. The full name of the console will be `library.name`.
* **SST_ELI_ELEMENT_VERSION(major, minorX, minorY)** This is a macro that specifies the version of the console. `major`, `minorX`, and `minorY` are integers that form a version number major.minorX.minorY. For example: SST_ELI_ELEMENT_VERSION(3, 0, 9) yields a version of 3.0.9. Versions are not checked by SST, this is provided for developers to version and manage their libraries.
* **description** (string) A description of the console

## Example

### Registering an InteractiveConsole
The name of the example console below will be `examples.console`.

```cpp title="exampleConsole.h"
#include <sst/core/interactiveConsole.h>

class ExampleConsole : public SST::InteractiveConsole
{
    public:

    // ELI macro to register console with SST-Core
    SST_ELI_REGISTER_INTERACTIVE_CONSOLE(
        ExampleConsole,                   // Console class
        "examples",                       // Library name, the 'lib' in SST's lib.name format
        "console",                        // Name used to refer to this console, the 'name' in SST's lib.name format
        SST_ELI_ELEMENT_VERSION(0, 1, 0), // A version number
        "A very simple console")          // Description

    // ... rest of class
}
```


## Header
```cpp
#include <sst/core/interactiveConsole.h>
```
