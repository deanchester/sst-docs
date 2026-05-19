---
title: Performance tips
---

The performance of SST is largely dependent on the elements used in simulation. Two simulations using different elements can have completely different performance characteristics. The performance of SST *in parallel* will be a function of both the elements used *and* the simulation graph characteristics combined with the partitioning.

## Improving element performance
An element's only function during SST's [Run stage](../concepts/lifecycle.mdx) is to process clocks and events. A given handler may be executed millions or billions of times. Streamlining event and clock handlers and ensuring clock handlers are not invoked more often than necessary goes a long ways towards efficient simulation. With that in mind, here are some suggestions for performance optimization.

SST also provides profiling infrastructure to assist in identifying performance bottlenecks. This can be used to track time in particular handler functions as well as the number of handler function invocations. Refer to [Profiling simulation performance](../features/profile.mdx) for details.

### Manage clocks
SST itself is event-driven but because it treats clock ticks as events, elements that register a clock will cause SST to execute in a time-stepped fashion. To avoid this, elements should allow SST to skip clock events by disabling their clocks *any time* they do not need to be executed. A common strategy for elements that receive events and queue them to process during a clock handler is to disable the clock when no events are in the queue and re-enable the clock when an event arrives. Depending on clock frequency and the complexity of the clock handler, this can lead to significant savings even if a clock is off for only a cycle or two at a time. 

```cpp
// A clock tick function
// Disables the clock if the queue is empty
bool clock(Cycle_t cycle)
{
    process(queue.front());
    queue.pop();
    if (queue.empty()) {
        clock_off = true;
        return true;
    }
    return false;
}

// An event handler function
// Re-enables the clock if it is not currently enabled
void event(SST::Event* ev)
{
    if (clock_off) {
        clock_off = false;
        reregisterClock(clock_frequency, clock_handler);
    }
    queue.push(ev);
}
```

Returning `true` from a clock handler function disables the clock without overhead. A clock can be re-enabled at any time using [`reregisterClock`](../../core/component/time/reregisterClock.md). The cost of re-enabling is a map lookup and vector append. Clocks can also be disabled at any time using [`unregisterClock`](../../core/component/time/unregisterClock.md) but this function has higher overhead than disabling via the clock handler return value as it performs a map lookup followed by a vector search and deletion.

### Limit output
Output functions (`SST::Output`, `printf`, etc.) will slow down simulation. We have frequently observed 5-15\% degradation when output functions are used regularly in element code. Using the `verbose` and `debug` capabilities in `SST::Output` helps but even calls that do not ultimately print can have overhead when executed many times. Use output sparingly in commonly executed functions or `#ifdef` output functions for higher performance. One method to do this is to wrap output calls in an `#ifdef __SST_DEBUG_OUTPUT__` which will enable them only when SST-Core is configured with `--enable-debug`.

### Pointer casting
When safe and possible to do so, use `static_cast` instead of `dynamic_cast` in event and clock handlers, particularly those that are called frequently. Over the course of many handler invocations, the overhead of `dynamic_cast` can become apparent.


## Improving parallel performance
Parallel performance can be improved both by changing the way the simulation elements are designed and by changing the way they are instantiated and partitioned during a particular simulation.

SST partitions Components across ranks and/or threads. SubComponents, modules, and other elements are always co-located with the Component to which they are attached (together referred to as a *Component tree*). Each Component tree processes events serially. Therefore, while it is possible to design an entire system as a single Component tree, doing so will cause your simulation to be completely serial. On the other end of the spectrum, one could go so far as to model individual logic gates as Components with Links representing the wires between them. This would yield a system with significant parallel opportunity but the overhead of event processing and synchronization would dwarf the advantage from the increased parallelism.

A second consideration is SST's synchronization. SST computes a synchronization interval between two partitions by computing the *minimum latency of the links that cross the partitions*. That latency determines how far the partitions can run ahead before needing to synchronize. Because SST is event driven, only relative latencies matter. Consider a system where every component is operating on a clock cycle of 1GHz (1ns) but the links have a latency of 100ps. Between each clock handler invocation on a Component, SST will have executed 10 synchronizations, many more than necessary. In that case, components on each partition can execute only a single clock cycle before needing to synchronize. If a clock cycle requires little work, the overhead of synchronization will dominate. In many cases, a clock cycle may require modest work - executing a clock handler and processing a few received events. Parallelizing the simulation will yield some speedup but the same simulation with a higher synchronization interval will likely yield more speedup as partitions will spend less time executing and waiting for synchronization.


## Design for parallel performance
Given the above, the following guidelines can help you determine how to model a system within SST. "High" and "low" latency means latency relative to other parts of the system.

### Guidelines for designing simulations
* Push latency to links: If you can equivalently stall events in a Component for X cycles before sending *or* send events on a link with an X cycle latency, it is better to model the latency in the link. In addition to the latency defined in the simulation input file, latency added to links prior to the `setup()` phase  will be considered when SST computes its synchronization intervals. Add latency to links using [`addRecvLatency()`](../../core/link/addRecvLatency.md) and/or [`addSendLatency()`](../../core/link/addSendLatency.md).
* If two parts of the simulated system need to communicate with very low latency, consider making them a single Component tree.
* Component trees cannot send zero latency events to each other.
* Disable clocks *any time* a component is idle and does not need to execute its clock handler. We have observed performance improvement even when disabling for one or two cycles at a time.
* Components and SubComponents have roughly similar memory overhead.

### Guidelines for developing simulation configurations
* Use the [`setNoCut()`](../../config/link/setNoCut.md) function to ensure that components connected with low-latency links are located on the same parallel partition. 

