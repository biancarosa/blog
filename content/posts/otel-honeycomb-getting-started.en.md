+++
title = "Getting started with OTEL & Honeycomb in Python"
description = "A practical guide to implementing OpenTelemetry with Honeycomb in Python applications"
tags = [
    "opentelemetry",
    "python",
    "honeycomb",
    "observability",
    "backend"
]
date = "2025-04-10"
categories = [
    "development"
]
author = "@__biancarosa"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/getting-started-with-otel-and-honeycomb).*

I used OpenTelemetry in my "lastfm-last-played" project - a playground project serving a simple API that shows what music a user last played through last.fm. This was a great opportunity to explore OTEL outside of a work environment, which helps understand concepts better because the project is small and easy to understand.

## Required Dependencies

First, add the necessary packages to your pyproject.toml (or requirements.txt/Pipfile):
- opentelemetry-api
- opentelemetry-sdk
- opentelemetry-instrumentation-flask
- opentelemetry-instrumentation
- opentelemetry-distro
- opentelemetry-exporter-otlp

## Auto-Instrumentation Setup

The simplest way to implement OTEL is with auto-instrumentation. You set up environment variables like:
- `OTEL_SERVICE_NAME`
- `OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf`
- `OTEL_EXPORTER_OTLP_ENDPOINT=https://api.honeycomb.io`

Then run with `opentelemetry-instrument`. This approach automatically instruments your application without requiring explicit code changes throughout your codebase.

## API Key Configuration

The last variable needed is `OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=<my-honeycomb-key>"`, which can be set in a .env file or directly on the shell (and in production app secrets).

## Observability Tips

Create meaningful spans: Break down complex operations into smaller spans to better understand where time is spent.

The full pull request with the implementation lives here: https://github.com/biancarosa/lastfm-last-played/pull/34

---

[Read the full post on Substack](https://backendengineeringadventures.substack.com/p/getting-started-with-otel-and-honeycomb)
