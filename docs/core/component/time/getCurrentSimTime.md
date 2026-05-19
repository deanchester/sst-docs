---
title: getCurrentSimTime
---

```cpp
SimTime_t getCurrentSimTime() const;
SimTime_t getCurrentSimTime(TimeConverter base) const;
SimTime_t getCurrentSimTime(const std::string& base) const;
SimTime_t getCurrentSimTime(const char* base) const;
```
*Availability:* Component, SubComponent, ComponentExtension

Returns the current simulation time as a cycle count. If a clock frequency is provided as either a TimeConverter or string, returns the cycle count in those units. Otherwise this function returns the cycle count in terms of the (sub)component's default timebase.

## Parameters
* **base** (TimeConverter, string, char*) The frequency or period of a clock to use when returning the number of cycles the simulation has been running. If a string or char*, units are required and can be SI units. For example "1ns", "200MHz", "3s", etc.
* **returns** (SimTime_t) Current simulation time as a cycle count in terms of either the clock frequency provided to the function, or if none is provided, the (sub)component's default timebase


## Example

<!--- SOURCE_CODE: None --->
```cpp
std::string period = "2ns";
output.output("For a clock period of 2ns, the cycle count is %" PRIu64 " cycles.\n", getCurrentSimTime(period));
```

## Header
```cpp
#include <sst/core/component.h> // or
#include <sst/core/subcomponent.h> // or
#include <sst/core/componentExtension.h>
```
