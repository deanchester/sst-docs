---
title: Default Serialization
---

The following objects have a default serialization method in SST. These objects can be serialized and deserialized simply by passing the object to the `SST_SER()` macro.

* All POD types (`bool`, `int`, etc.) and `std::string`
* All standard library containers (`std::vector`, `std::map`, `std::array`, etc.)
* Some standard library types: `std::atomic`, `std::pair`, `std::tuple`, `std::unique_ptr`
* SST types: `Link`, `TimeConverter`, `Output`, `RNG:Random`, `RNG:RandomDistribution`, `SharedArray`, `SharedMap`, `SharedSet`, `UnitAlgebra`, `Statistic`, `StatisticOutput`
* SST Handlers: `Clock::Handler`, `Event::Handler`
* SST Interfaces: `SimpleNetwork`, `StandardMem`
* C-style arrays (using array wrapper: `SST_SER(SST::Core::Serialization::array(arr, size))`)
* Pointers to any of the above


## See also: [Checkpoint Guide](../../guides/features/checkpoint.mdx).
