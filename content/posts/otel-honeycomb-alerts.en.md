+++
title = "OpenTelemetry, Python & Honeycomb: Setting Up Alerts"
description = "How to configure meaningful alerts with OpenTelemetry and Honeycomb"
tags = [
    "opentelemetry",
    "python",
    "honeycomb",
    "observability",
    "backend"
]
date = "2025-06-09"
categories = [
    "development"
]
author = "@__biancarosa"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/opentelemetry-python-and-honeycomb).*

This post follows my previous one about setting up OTEL in Python's API. Now let's focus on how to use this data we're sending to have useful alerts - not just dashboards, since no one wants to go insane refreshing them all the time.

## Alerts in Honeycomb

In Honeycomb, alerts are built around triggers - queries that run periodically and notify you when certain conditions are met. For example, monitoring HTTP 500 errors to catch issues in production.

## Key Considerations

Alert configuration involves two main aspects:
- **Query Configuration:** Defining what you're monitoring
- **Alert Thresholds:** When to trigger notifications

These settings will likely need adjustment after initial setup. It requires knowing your system, trial and error, and observing historical patterns. Dynamic baselines can be extremely helpful if you know what you're doing.

## Learning from False Positives

After setting up alerts, my Slack channel got filled with notifications happening at least twice a day. However, the ratio of 500s to 200s wasn't significant. Deeper investigation of traces revealed the failures occurred due to errors querying the upstream API - something outside my control.

The solution? Implement a 504 status code for upstream timeouts and exclude these from alerts. For more critical products, you might want to create a second, more specific alert like "we're receiving too many errors from the upstream API" rather than a generic "too many errors" alert.

---

[Read the full post on Substack](https://backendengineeringadventures.substack.com/p/opentelemetry-python-and-honeycomb)
