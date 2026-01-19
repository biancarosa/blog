+++
title = "Modular Monoliths: The Architecture Pattern We Don't Talk Enough About"
description = "Exploring modular monoliths, module communication, and interface placement strategies"
tags = [
    "architecture",
    "python",
    "backend",
    "modular-monolith"
]
date = "2026-01-12"
categories = [
    "development"
]
author = "@__biancarosa"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/modular-monoliths-the-architecture).*

We need to talk more about Modular Monoliths. Using a Python app as an example, this is just a way to organize folders, so it is a software architecture pattern that is transferable to many languages.

## Module Communication

The key to maintaining modularity is controlling how modules interact. Modules should communicate through service interfaces, not by directly importing each other's models or internals.

## Two Approaches for Interface Placement

1. **Routes within modules:** A particular module can have its own routes defined right in the module folder. If and when you break things into a new app, you just port it all into it, including API layers, and voila - you have a new service!

2. **Framework-free modules:** For new apps where technical decisions are still being defined, keeping modules framework-free might be better. It allows flexibility to decide between framework/transport patterns of API layers when porting.

## Framework Independence

Keeping the modules framework-free means the service layer doesn't import Flask, FastAPI, or any web framework directly. Services return plain Python objects or raise regular exceptions, which the route layer then transforms into HTTP responses. This way, you can switch from Flask to FastAPI (or to a gRPC service) without touching your business logic.

## Business Logic Placement

Routes/controllers shouldn't have much business logic - all business logic should be in the service layers.

---

[Read the full post on Substack](https://backendengineeringadventures.substack.com/p/modular-monoliths-the-architecture)
