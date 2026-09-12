+++
title = "Putting at test how portable OpenTelemetry really is"
description = "Can I move from a provider to another without code changes? My journey of exploring a migration on one of my projects."
date = "2025-12-23T14:48:56+00:00"
author = "@__biancarosa"
tags = ["observability", "railway", "monitoring", "open telemetry", "otel"]
categories = ["development"]
substack_url = "https://backendengineeringadventures.substack.com/p/putting-at-test-how-portable-opentelemetry"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/putting-at-test-how-portable-opentelemetry).*

I posted a little while ago on [how to set up OTel in Python and send data to Honeycomb](https://backendengineeringadventures.substack.com/p/getting-started-with-otel-and-honeycomb). I love Honeycomb, but my requirements for observability tooling on personal projects changed - I wanted to self-host because I had more services to plug in and didn't want to pay a service subscription (because this is all my playground).

So I decided to give Grafana a shot. Good for learning purposes since I’d never used it extensively. I deployed the stack (Loki, Prometheus, Tempo, Grafana) on [Railway](https://railway.com/?referralCode=sWx4lg), along with an OTEL Collector, to serve as the central hub. Initially, I used this [template,](https://railway.com/new/template/8TLSQD) but I ended up creating my [own](https://railway.com/deploy/YqPTd9?referralCode=sWx4lg&utm_medium=integration&utm_source=template&utm_campaign=generic) because the first one didn't include the OTel Collector. I can explore more about how I did it!

**So, was it easy to port things over?**

Yes. Surprisingly so.

The application code? Untouched. The only change was swapping one environment variable:

```
# Before (Honeycomb)
OTEL_EXPORTER_OTLP_ENDPOINT=https://api.honeycomb.io

# After (Self-hosted)
OTEL_EXPORTER_OTLP_ENDPOINT=http://opentelemetry-collector-contrib.railway.internal:4318
```

That’s it. The `opentelemetry-instrument` auto-instrumentation kept working exactly as before. Traces and metrics started flowing to Tempo and Prometheus immediately.

[![](https://substackcdn.com/image/fetch/$s_!TjyH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9220fcc9-17f1-4051-8a3d-6216f3d02ef0_2166x1384.png)](https://substackcdn.com/image/fetch/$s_!TjyH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9220fcc9-17f1-4051-8a3d-6216f3d02ef0_2166x1384.png)

**Where I actually struggled**

The migration itself was smooth. What tripped me up was properly configuring the Grafana stack and correctly setting up the OTEL Collector endpoints.

Once I fixed those, everything worked. No application changes needed.

**The verdict**

OpenTelemetry delivered on its portability promise<strong>. Switching from Honeycomb to a self-hosted stack required exactly one environment variable change and zero modifications to my instrumentation code.</strong>

The only gotcha is that auto-instrumentation doesn’t capture logs - you’ll need a few lines of manual configuration for that, regardless of your backend. So I did that, and bam - log lines now exist on Loki and can be seen on my Grafana dashboard! [Here is the link to check the code.](https://github.com/biancarosa/lastfm-last-played/blob/main/api/app/main.py#L33)

**Will it always be easy to switch providers, even in a big company?**

No, it won't.

I started sending data to the new provider, but if there are alerts, dashboards, notification channels, users, and teams set up, all of those follow each stack's syntax and format. Depending on your company size, this is likely where the pain will lie.

**Lit! I want this!**

If you want to spin up your own Grafana observability stack on Railway, I’ve open-sourced my setup: [railway-grafana-stack](https://github.com/biancarosa/railway-grafana-stack). It includes the OTEL Collector, Tempo, Loki, Prometheus, and Grafana - all pre-configured to work together. In another post, I can talk about how I manage to spin all of them in a private network and connect to my Grafana dashboard on my own machine - no public endpoints involved!
