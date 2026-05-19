---
title: SST_ELI_REGISTER_PARTITIONER
sidebar_label: Partitioner
---

```cpp
SST_ELI_REGISTER_PARTITIONER(class_name, "library", "name", 
    SST_ELI_ELEMENT_VERSION(major, minorX, minorY), "description")
```

All partitioners must register themselves with SST using this macro. The library and name strings provided in this macro 
will be used by SST to identify the partitioner as "library.name". The version, description, and category are displayed 
by `sst-info` to document the purpose and version of the partitioner.

:::info Important
This macro must reside in a `public` section of the partitioner's header file.
:::


## Parameters

* **class_name** (class) The name of the partitioner class. This is not a string.
* **library** (string) The name of the library that this partitioner belongs to. If the library name does not exist, it will be created.
* **name** (string) The name that will be used to instantiate this partitioner in the simulation input configuration. It can be the same as the class_name but does not need to be. The full name of the partitioner will be `library.name`.
* **SST_ELI_ELEMENT_VERSION(major, minorX, minorY)** This is a macro that specifies the version of a partitioner. `major`, `minorX`, and `minorY` are integers that form a version number major.minorX.minorY. For example: SST_ELI_ELEMENT_VERSION(3, 0, 9) yields a version of 3.0.9. Versions are not checked by SST, this is provided for developers to version and manage their libraries.
* **description** (string) A description of the partitioner

:::info
`library` and `name` must follow SST's [element naming conventions](../../../guides/dev/naming.md).
:::

## Example

```cpp
class SimplePartitioner : public SST::Partition::SSTPartitioner
{
public:

    SST_ELI_REGISTER_PARTITIONER(
        SimplePartitioner,              // Partitioner class
        "example",                      // Library name, the 'lib' in SST's lib.name format
        "simple",                       // // Name used to refer to this partitioner, the 'name' in SST's lib.name format
        SST_ELI_ELEMENT_VERSION(1,0,0), // A version number
        "Simple partitioning scheme"    // Description
    )

/* Rest of class */
};

```

## Header
```cpp
#include <sst/core/sstpart.h>
```