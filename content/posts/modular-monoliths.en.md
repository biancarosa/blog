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
substack_url = "https://backendengineeringadventures.substack.com/p/modular-monoliths-the-architecture"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/modular-monoliths-the-architecture).*

Microservices can be great. Monoliths can be great, too. Both serve their purpose and, when used right, can power incredible products and systems.

I’m not gonna talk about how to build a microservice, or when to do so. I’m pretty sure you all got enough of this around the internet. But we need to talk more about Modular Monoliths, so I’m gonna share how I build them.

# The Anatomy of a Modular Monolith

I’ll use a Python app as an example, but this is just a way to organize folders, so it is a software architecture pattern that is transferable to many languages.

Here's what it looks like:

```
my-python-api/
├── orders/
│   ├── models/
│   ├── services/
│   ├── __init__.py
├── payments/
│   ├── models/
│   ├── services/
│   ├── __init__.py
├── notifications/
│   ├── models/
│   ├── services/
│   ├── __init__.py
├── common/
|   ├── date_utils.py
|   ├── string_utils.py
└── main.py
```

## Module Communication Rules

The key to maintaining modularity is controlling how modules interact. Modules should communicate through service interfaces, not by directly importing each other’s models or internals. For example, if `orders` needs payment information, it should call `PaymentService.get_payment()` rather than import the `Payment` model directly. This keeps boundaries clear and makes future extraction easier because payments and orders might not even live in the same database.

### What to do with routes (or blueprints, or controllers?)

Whatever you call those interfaces, it’s always hard to decide where to place them in a modular monolith. You normally have two options:

### 1 - Have each module declare its own interfaces

This is normally my preferred method when the frameworks to be used are well-defined, established, unlikely to change, etc. It means that a particular module can have its own routes defined right in the module folder, and if and when you break things into a new app, you just port it all into it, including API layers, and voilá, you have a new service!

```
orders/
├── models/
├── services/
├── routes/
│   ├── main.py
│   ├── create.py
│   ├── read.py
│   ├── update.py
│   ├── delete.py
└── main.py
```

Or something like

```
orders/
├── models/
├── services/
├── gql/
│   ├── mutations.py
|   ├── queries.py
└── main.py
```

### 2 - Place it all on the main app, and use services as the main communications interface

For new apps, the technical decisions are still getting defined, so I normally start with this approach and then eventually move to (1) if necessary. In fast-paced environments, this might be a better solution - and it allows you to keep your modules framework-free, which is ALWAYS good - because if you need to port it, you will have some flexibility to decide between framework/transport patterns of your API layers. It is highly likely that it’ll need to change at least a bit.

Keeping the modules framework-free means your service layer doesn’t import Flask, FastAPI, or any web framework directly. Services return plain Python objects or raise regular exceptions, which the route layer then transforms into HTTP responses. This way, you can switch from Flask to FastAPI (or to a gRPC service) without touching your business logic.

And ideally, your routes/controllers shouldn’t have much business logic anyway - all business logic should be in the service layers (I’m against models with logic, but that’s another topic!) - so in my opinion, this is not a bad choice at all.

```
my-python-api/
├── orders/
│   ├── models/
│   ├── services/
│   ├── __init__.py
├── routes/
│   ├── orders.py
└── main.py
```

But be careful here!!!!

main imports routes → ✅
routes imports main - ❌ NO!

This creates a circular dependency that makes the code harder to test, refactor, and understand. Routes should only import from your modules (orders, payments, etc.), never from the main application file.

[![](https://substackcdn.com/image/fetch/$s_!0O_h!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feff85e97-59a8-4a61-9af2-1b5e5c692128_1182x841.png)](https://substackcdn.com/image/fetch/$s_!0O_h!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feff85e97-59a8-4a61-9af2-1b5e5c692128_1182x841.png)

### When to migrate from approach 2 to approach 1?

I typically move from centralized routes to module-owned routes when:

- The module has grown to 10+ endpoints
- Multiple teams start working on different modules
- Seriously considering extracting the module to its own service within 3-6 months
- The module needs specific middleware or authentication that differs from others and is unique to specific module code

# Next posts in this series

I currently have a few more posts planned out, and I'll share them on a weekly basis here:

- Deploying Modular Monoliths: One Codebase, Multiple Services
- Database Design for Modular Monoliths: Avoiding the Coupling Trap
- Reality Check: What Modular Monoliths Don’t Solve
- Building a Modular Monolith: Complete Example And What To Do When Things Get Tricky

Please comment out if you have questions or would like to know how to architect this type of system in a specific scenario, and I’m happy to either answer in one of the posts or add more posts in the series!
