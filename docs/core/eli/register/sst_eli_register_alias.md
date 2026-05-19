---
title: SST_ELI_REGISTER_ALIAS
sidebar_label: Alias
---

```cpp
SST_ELI_REGISTER_ALIAS("name")
```

This macro can be used to register an alias for a named element. Both the name registered under the element registration macro (e.g., `SST_ELI_REGISTER_COMPONENT`) and with this macro will point to the same element. Note that `sst-info` will display the element under its non-aliased name but will show the alias as an alternative. This macro is especially useful for changing an element name without breaking existing configuration files. The old name should be registered using the alias macro and the new name should be used in the element registration macro.

:::info Important
This macro must reside in a `public` section of the element's header file.
:::


## Parameters

* **name** (string) An alias name that can be used to instantiate this element in the simulation input configuration.

:::info
`name` must follow SST's [element naming conventions](../../../guides/dev/naming.md).
:::

## Example

```cpp
class example1 : public SST::Component
{
public:

    SST_ELI_REGISTER_COMPONENT(
        example1,                           // Component class
        "simpleElementExample",             // Component library (for Python/library lookup)
        "example1",                         // Component name (for Python/library lookup)
        SST_ELI_ELEMENT_VERSION(1,0,0),     // Version of the component (not related to SST version)
        "Example #2, statistics & RNG",     // Description
        COMPONENT_CATEGORY_UNCATEGORIZED    // Category
    )

    //highlight-next-line
    SST_ELI_REGISTER_ALIAS(
        "component_example"                 // Alias name that can be used in place of the component name above
    )

/* Rest of class */
};
```

The above would allow loading the component by the name `simpleElementExample.example1` or `simpleElementExample.component_example`.

## Header
This macro is available alongside the `SST_ELI_REGISTER_*` macros for each element type. See the individual ELI registration macro documentation for the needed header.