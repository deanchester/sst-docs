---
title: SST::TimeConverter
---

:::warning Deprecation
Shared TimeConverters returned by SST-Core APIs are removed in SST 16.0 after being deprecated in SST 15.0. All functions that previously accepted a pointer now accept an object. Functions that returned a pointer now return an object. Elements expecting TimeConverter pointers must be updated accordingly.
:::


A TimeConverter is an object used to manage the conversion of time between global time and various local (Component/SubComponent) views of time. See [Time in SST](../../guides/concepts/time) for a more detailed discussion of how SST handles time.

TimeConverters can be created by calling certain (Sub)Component functions. These are:
* [registerClock](../component/time/registerClock)
* [registerTimeBase](../component/time/registerTimeBase)
* [getDefaultTimeBase](../component/time/getDefaultTimeBase)
* [getTimeConverter](../component/time/getTimeConverter)

Components should use the above methods instead of directly constructing TimeConverters. These functions perform additional error checking such as ensuring that TimeConverters with smaller periods than the simulation's timebase are not used.