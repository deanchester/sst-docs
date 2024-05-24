---
title: Glossary of SST Terms
---

## Anonymous SubComponent
A SubComponent that is not directly instantiated in the simulation config file but rather instantiated by the loading Component or SubComponent.

## AutoTester
SST's continuous integration (CI) and testing infrastructure.

## Clock
An event that is triggered on a regular cadence (frequency) by calling a clock handler.

## Component
The basic building block of an SST simulation. Components are individual simulation models and multiple components can be connected to form simulated systems.

## ComponentExtension
A class that is part of a Component. Used when it makes sense from a code-organization perspective to break a Component into multiple classes.

## Core
Generally refers to "SSTCore", SST's discrete-event simulation framework.

## Config file
The input provided to SST which defines the components to simulate, how to connect those components, and what statistics should be generated by the simulation. Most often this is a Python script but other inputs such as JSON are possible too.

## Element
A dynamic library containing a collection of components, subcomponents, etc.

## ELI
"Element Library Information". Metadata including name, description, supported parameters, ports, etc. about an SST object (Component, SubComponent, Module) that is registered with the SSTCore.

## Event
The unit of communication between Components.

## Link
Links represent connectivity between components. Components interact with other components by sending Events over Links.

## Module
A dynamically-loadable class that does not have access to SST's Component APIs (see SubComponent for contrast).

## Port
A named connection point for a Link in a Component or SubComponent.

## Self-link
A special case of Link where both endpoints of the Link connect to the same Component or SubComponent.

## SubComponent
A class that can be dynamically loaded into a Component or another SubComponent and that has access to SST's Component APIs. SubComponents are considered part of their parent (Sub)Component and may communicate with their parent or child (Sub)Components via function calls in addition to Links.

## Time vortex
The data structure in SSTCore that manages event delivery and ordering.

## User SubComponent
A SubComponent that is explicitly instantiated in an SST configuration file.