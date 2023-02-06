---
title: SubComponent constructor
---

```cpp
// Subclass constructor
SubComponentClassName(SST::ComponentId_t id, SST::Params& params, ARGS ...);
// Base SST::SubComponent class constructor
SST::SubComponent::SubComponent(SST::ComponentId_t id);
```
*Availability*: SubComponent
This constructor is called when a Sub(Component) loads a new SubComponent.

## Parameters
* **id** (ComponentId_t) A unique ID generated by SST for each SubComponent. 
* **params** (Params&) The parameter set passed into the SubComponet by the simulation configuration file if user-defined or by the parent (Sub)Component if anonymous.
* **...** (variable) Variable arguments depending on the specific SubComponent definition
* **returns** (SubComponent) The newly constructed SubComponent

## Examples

<!--- SOURCE_CODE: sst-elements/src/sst/elements/simpleElementExample/basicSubComponent_subcomponent.h --->
```cpp
/* simpleElementExample/basicSubComponent_subcomponent.h */
#include <sst/core/subcomponent.h>

// SubComponent API - define an API for a type of subcomponent
class basicSubComponentAPI : public SST::SubComponent 
{
public:
    // Tell SST that this class is a SubComponent API
    SST_ELI_REGISTER_SUBCOMPONENT_API(SST::simpleElementExample::basicSubComponentAPI)

    basicSubComponentAPI(ComponentId_t id, Params& params) : SubComponent(id) {}
    virtual ~basicSubComponentAPI() {}

    virtual int compute (int num) =0;
    virtual std::string compute (std::string comp) =0;
};
```

## Header
```cpp
#include <sst/core/subcomponent.h>
```