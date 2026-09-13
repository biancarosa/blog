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
substack_url = "https://backendengineeringadventures.substack.com/p/getting-started-with-otel-and-honeycomb"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/getting-started-with-otel-and-honeycomb).*

Let's look at how I used OpenTelemetry in my [lastfm-last-played](http://github.com/biancarosa/lastfm-last-played) project, which is a playground project of mine that serves a simple API that shows what music a user last played through last.fm. I’ve started getting a few more requests over the last months, and this made me want to monitor some things:

- How often each user's data is queried
- Response times for each component of the request
- Error rates when last.fm is down or rate-limits our requests

Of course, this was also a great opportunity to explore OTEL (Open Telemetry) outside of a work environment, which always helps me understand the concepts better, because this project of mine is very small and easy to understand.

---

# The Basics

First, I've had to add the necessary packages to my *pyproject.toml*. This can be your *requirements.txt, Pipfile,* or an equivalent file that lists your project's dependencies.

```
opentelemetry-api

opentelemetry-sdk

opentelemetry-instrumentation-flask

opentelemetry-instrumentation

opentelemetry-distro

opentelemetry-exporter-otlp
```

Then install the dependencies. If using pip, you can do it with:

```
 pip install -e .
```

**Instrumentation with Docker**

The simplest way to implement OTEL is with auto-instrumentation. Here's how I configured it in my `Dockerfile`:

```
# Set up OpenTelemetry environment variables

ENV OTEL_SERVICE_NAME=lastfm-last-played

ENV OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf

ENV OTEL_EXPORTER_OTLP_ENDPOINT="https://api.honeycomb.io"

# Run with OpenTelemetry instrumentation

CMD ["opentelemetry-instrument", "gunicorn", "--bind", "0.0.0.0:8000", "app.main:app"]
```

This approach automatically instruments your application without requiring explicit code changes throughout your codebase. The `opentelemetry-instrument` wrapper sets up the necessary instrumentation for common libraries and frameworks.

Since we're using Flask, the fact that we have `opentelemetry-instrumentation-flask` is installed also gives us some instrumentation for free.

The last variable I needed to set up, in a .env file or directly on the shell (and on my production app secrets), was

```
OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=<my-honeycomb-key>"
```

[![](https://substackcdn.com/image/fetch/$s_!jU-S!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffed6f95d-979f-4171-a82b-9416f7d376cc_2336x1700.png)](https://substackcdn.com/image/fetch/$s_!jU-S!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffed6f95d-979f-4171-a82b-9416f7d376cc_2336x1700.png)

This is pretty cool because I am now getting visibility over my environment and sending data in an *open-source* format, which means I am not vendor locked and can switch vendors anytime. I also don't need to learn how to instrument my app with library x or y from vendor x or y all the time. That is why I am a huge fan of open source standards and will always vouch for them.

---

---

# Answering more specific questions

But I needed some very specific questions answered: Who are my app's users? How many unique users do I have? For that, I needed some custom instrumentation.

**Setting up the tracer**

Here's how I implemented tracing in my API's endpoint:

```
def route(user):
    with tracer.start_as_current_span("get_latest_song") as span:
        span.set_attribute("user", user)
```

This bit of code starts a **span** and adds the **user** as a span attribute. Since this is the first span in the route, it is also the **parent span.**

Then I could build a pretty dashboard of unique users.

[![](https://substackcdn.com/image/fetch/$s_!cuGf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a75571a-98ba-4b53-a263-aabf352a7421_2316x1732.png)](https://substackcdn.com/image/fetch/$s_!cuGf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a75571a-98ba-4b53-a263-aabf352a7421_2316x1732.png)

<strong>More spans, please!</strong>If you look at my code [here](https://github.com/biancarosa/lastfm-last-played/blob/main/api/app/songs/latest_songs.py#L25), you will see that I created even more *child* spans. Why?

I needed visibility and wanted to know, for each request (*get_latest_song*):

- If there were errors while getting the API key (*check_api_key*)
- If the last.fm request failed or not, and some data like how long it took and the status code of it (*lastfm_api_request*)
- If my logic was parsing the Last.FM data incorrectly or doing something weird/sketchy (*process_response*)

So now, when I inspect a trace, I see…

[![](https://substackcdn.com/image/fetch/$s_!h5-Y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F40030883-98ba-494b-b9df-6c12d1cb499b_2302x1740.png)](https://substackcdn.com/image/fetch/$s_!h5-Y!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F40030883-98ba-494b-b9df-6c12d1cb499b_2302x1740.png)

Yay! 🚀

---

# Check out the code!

The full pull request lives here: [https://github.com/biancarosa/lastfm-last-played/pull/34,](https://github.com/biancarosa/lastfm-last-played/pull/34) so feel free to check it out on GitHub.

---

# Best Practices According To My Experience

Based on my experience when it comes to observability, here are some cool tricks I would like to share:

1. **Create meaningful spans**: Break down complex operations into smaller spans to better understand where time is spent. I created separate spans for API key validation, external API requests, and response processing.

2. **Add context with attributes**: Include relevant information in your spans to make debugging easier. In my implementation, I added attributes like *user* and *lastfm.status_code* to provide context about the request.

3. **Use status codes and exceptions properly**: When exceptions occur, set the span status to ERROR and record the exception with *span.record_exception(exception) .* This creates a direct link between errors and their traces.

4. **Structure your instrumentation**: In `latest_songs.py`, I created a main span for the whole operation with sub-spans for specific parts, making it easy to see which parts of the code are taking time.

If you have any other tips, feel free to add your comment and let's start a discussion! I'd love to learn what the things you keep in mind when setting up observability for your apps.

---

## Thank you for reading!

If you got all the way here, I hope this was useful!

Stay tuned for other backend adventures of mine in this substack 😊
