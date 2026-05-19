---
title: registerClock
---

```cpp
TimeConverter registerClock(const std::string& freq, Clock::HandlerBase* handler, bool reg_all = true);
TimeConverter registerClock(const UnitAlgebra& freq, Clock::HandlerBase* handler, bool reg_all = true);
TimeConverter registerClock(const TimeConverter freq, Clock::HandlerBase* handler, bool reg_all = true);
```
*Availability:* Component, SubComponent, ComponentExtension

Register a clock at the requested frequency or period. On each clock cycle, the associated handler will be called. Unless otherwise specified, this call wil also sets the default time base for the (sub)component to match the clock frequency. The frequency parameter can be specified as a frequency or period (e.g., "1GHz" and "1ns" are interchangeable).


## Parameters
* **freq** (string, UnitAlgebra, TimeConverter) Frequency or period of the clock
* **handler** (Clock::HandlerBase*) Clock handler function to invoke each cycle
* **reg_all** (bool) Whether to set the (sub)component's default timebase to this clock frequency
* **returns** (TimeConverter) A time converter representing the clock frequency


## Example

<!--- SOURCE_CODE: sst-elements/src/sst/elements/simpleElementExample/example0.cc --->
```cpp title="Excerpt from sst-elements/src/sst/elements/simpleElementExample/example0.cc"
#include <sst/core/component.h>

example0::example0(ComponentId_t id, Params& params) : Component(id)
{
    /** Other configuration here */

    registerClock("1GHz", new Clock::Handler<example0, &example0::clockTic>(this));

    /** Other configuration here */
}
```

## Header
```cpp
#include <sst/core/component.h> // or
#include <sst/core/subcomponent.h> // or
#include <sst/core/componentExtension.h>
```
