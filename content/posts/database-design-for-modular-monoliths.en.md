+++
title = "Database Design for Modular Monoliths: Avoiding the Coupling Trap"
description = "Post 3 of a series of 5 posts about one of my favorite techniques for when it's not time to break a monolith into microservices :)"
date = "2026-01-26T16:22:57+00:00"
author = "@__biancarosa"
tags = ["modern apps", "database", "microservices", "backend engineering", "software architecture", "modular monoliths", "backend", "monoliths"]
categories = ["development"]
substack_url = "https://backendengineeringadventures.substack.com/p/database-design-for-modular-monoliths"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/database-design-for-modular-monoliths).*

Last week, we covered deployment patterns - how to run multiple services from the same codebase. But here’s the thing: you can have perfectly separated Docker containers and still end up with a nightmare when it’s time to extract a module. Why? The database.

This is where most modular monoliths, migration to microservices or service extraction fails. What starts with `JOIN payments.transactions ON orders.payment_id = payments.id` ends up becoming the coupling hell - suddenly extracting the payments module requires untangling months of accumulated database coupling.

I’ve once watched as multiple teams struggled to break down core business logic out of a big database over the course of not one or two years, but 3+ years. When the company grows, multiple teams depend on the database schema, we normally curse those to started it for making it such coupled architecture decision so next time you see yourself building a foundation, don’t be the someone another engineer will hate!

**The Foreign Key Trap**

Cross-module relationships should be minimal. When the `orders` module needs to reference a customer, store `customer_id` as a simple UUID rather than creating a foreign key constraint. Why?

1. **Future extraction becomes trivial** - When you extract customers into its own service, you won’t need to untangle database constraints. The `customer_id` just becomes an external reference.
2. **Testing in isolation** - You can test the orders module without needing the customers tables to exist.
3. **Independent deployment** - If these become separate services, they can have completely different databases.

Here’s what I mean:

```
-- Avoid this (tight coupling via foreign key)
CREATE TABLE orders (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id)  -- FK creates coupling
);

-- Prefer this (loose coupling via ID reference)
CREATE TABLE orders (
    id UUID PRIMARY KEY,
    customer_id UUID  -- Just store the ID, validate in application layer
);
```

“But what about referential integrity?” I hear you. Yes, you lose the database-level guarantee that every `customer_id` points to a real customer. To use this strategy, you can simply **validate in the application layer.** Before creating an order, call `CustomerService.exists(customer_id)`. Simple, explicit, and keeps your modules talking through service interfaces like they should.

**Using Different Database Schemas**

Even better than just avoiding foreign keys: use separate database schemas from the start. This is my preferred approach when the database supports it.

```
CREATE SCHEMA customers_schema;
CREATE SCHEMA orders_schema;
CREATE SCHEMA notifications_schema;

CREATE TABLE customers_schema.customers (...);
CREATE TABLE orders_schema.orders (...);
```

In your application config (SQLAlchemy example):

```
# customers/models/base.py
class CustomerModel(Base):
    __tablename__ = 'customers'
    __table_args__ = {'schema': 'customers_schema'}

# orders/models/base.py
class OrderModel(Base):
    __tablename__ = 'orders'
    __table_args__ = {'schema': 'orders_schema'}
```

This approach gives you:

- **Clear boundaries** - You can actually *see* which tables belong to which module
- **Easy extraction** - Dump the schema, create a fresh migration in your new service with that state, done
- **Permissions control** - Different modules can have different database users with access only to their schemas

If your modules need to query across schemas (which should be rare), you can still do it during the monolith phase:

```
-- Still works in monolith, breaks when DBs are separated (which is what you want!)
SELECT o.*, c.email
FROM orders_schema.orders o
JOIN customers_schema.customers c ON o.customer_id = c.id;
```

This query naturally stops working when you separate services, forcing you to implement proper service-to-service communication instead of sneaky database joins.

**When This Gets Tricky**

Some scenarios genuinely need cross-module database access:

- **Reporting** - Your analytics dashboard needs data from everywhere
- **Search** - Full-text search across orders, customers, and products
- **Audit trails** - Compliance requires logging across modules

For reporting, consider a read replica + any sort of ETL that pipes data into Snowflake. There you can do all the view you need cross-joining all source DBs. For search, use a dedicated search service (Elasticsearch, etc.) that modules publish to. For audit trails, event sourcing patterns work well - each module emits events to a central audit log.

The key is being intentional. Cross-module database access isn’t forbidden - it just needs to be explicit and contained.

---

Next week, we’ll get into the reality check: what modular monoliths *don’t* solve, and when you should just bite the bullet and go with microservices from the start.

*Have you dealt with database coupling nightmares? What patterns worked (or didn’t work) for your team? Drop a comment below!*
