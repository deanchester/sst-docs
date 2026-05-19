---
title: carcosa
---

The *carcosa* element is designed to model complex, safety-critical compute systems—especially highly heterogeneous setups. Carcosa focuses on modeling how injected faults propagate through a system. Through a custom MMIO control protocol (hyades.h), Carcosa allows RISC-V guest binaries running on the [Vanadis](../vanadis/intro.md) processor model to actively participate in host-driven action loops. The flow of data between sensors, CPUs and memory/injected faults are handles by the Hali component. Hali coordinates with [Vanadis](../vanadis/intro.md) processes to actively intercept MMIO regions to keep multiple cores synchronized and coordinate their execution.

:::note At a Glance

**Source Code:** [sst-elements/.../carcosa](https://github.com/sstsimulator/sst-elements/tree/master/src/sst/elements/carcosa) &nbsp;  
**SST Name:** `carcosa` &nbsp;  
**Maturity Level:** Prototype (2) &nbsp;  
**Development Path:** Active &nbsp;   
**Last Released:** SST 16.0

:::

### Required dependencies
*None* 

### Optional dependencies
*None* 
