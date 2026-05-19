---
title: isStatisticShared
---

```cpp
bool isStatisticShared(const std::string& statistic_name, bool include_me = false);
```
*Availability:* Component, SubComponent, ComponentExtension

Determine whether the named statistic is shared with our parent. Returns false if the caller is a Component and `include_me` is false.

## Parameters
* **statistic_name** (string) Name of the statistic
* **include_me** (bool) Whether to count the calling component as a sharer
* **returns** (bool) Whether statistic is shared

## Example

<!--- SOURCE_CODE: None --->
```cpp
bool shared = isStatisticShared("num_events_received");
```

## Header
```cpp
#include <sst/core/component.h> // or
#include <sst/core/subcomponent.h> // or
#include <sst/core/componentExtension.h>
```
