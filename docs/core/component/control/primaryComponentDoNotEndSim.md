---
title: primaryComponentDoNotEndSim
---
```cpp
void primaryComponentDoNotEndSim();
```
*Availability*: Component, SubComponent, ComponentExtension

A primary (sub)component that has previously registered using [registerAsPrimaryComponent()](registerAsPrimaryComponent) calls this function to let the simulation know that simulation should not end until this component changes its state using [primaryComponentOKToEndSim()](primaryComponentOKToEndSim). After registering as primary, components will be in the ok-to-end state until this function is called.

If SST detects that all primary components are in the ok-to-end state (i.e., each primary component has either never called `primaryComponentDoNotEndSim()` or has most recently called `primaryComponentOKToEndSim()`), then simulation will end. A component may use alternating calls to `primaryComponentDoNotEndSim()` and `primaryComponentOKToEndSim()` to switch between states throughout simulation.

## Parameters
* **returns** None

## Example

<!--- SOURCE_CODE: sst-elements/src/sst/elements/simpleElementExample/basicSimLifeCycle.cc --->
```cpp title="sst-elements/src/sst/elements/simpleElementExample/basicSimLifeCycle.cc"
basicSimLifeCycle::basicSimLifeCycle( SST::ComponentId_t id, SST::Params& params ) : SST::Component(id) 
{
	// Register as primary and prevent simulation end until we've received all the events we need
	registerAsPrimaryComponent();
	primaryComponentDoNotEndSim();
}
```

## Header
```cpp
#include <sst/core/component.h> // or
#include <sst/core/subcomponent.h> // or
#include <sst/core/componentExtension.h>
```