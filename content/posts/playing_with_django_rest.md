+++
title = "Playing with Django Rest Framework"
description = ""
tags = [
    "python",
    "django",
    "web",
    "development",
    "en"
]
date = "2018-03-17"
categories = [
    "development",
    "en"
]
highlight = "true"
+++

This is the first of a series of posts that I will write to share some knowledge I gained on Python Web Frameworks plus a nice, public way to push me into learning more and more about them.

I’ve decided to do the same project on all of them, starting with Django Rest Framework. This is will be a standalone analysis and then I will move forward to comparisons. My intention is to find a good framework to plug into a database and see how well it handles GET requests.

I managed to do my setup with Django Rest Framework in about 15 minutes, and that’s because I rarely choose Django and I always go slow and reading lots of documentation.

I have to confess that I was overwhelmed by the web browseable API.

![Image of Django Rest Framework Web Browseable API](../../img/django-rest-1.png)

My code looks like that:

<script src="https://gist.github.com/biancarosa/6fe135f4165affa837e9e99237f09a41.js"></script>

Pretty neat, huh?

I was quite happy with the result and how fast it was to see it running. But let’s see how this baby works under pressure.

Running under [gunicorn](https://github.com/benoitc/gunicorn):

`gunicorn dashboard_api.wsgi`

I use [vegeta](https://github.com/tsenart/vegeta) for HTTP loading tests because it’s fucking awesome.

`vegeta attack -duration=30s -rate=200 -targets=targets.vegeta | vegeta report > report.vegeta`

And the results are:

```
Requests [total, rate] 6000, 200.03
Duration [total, attack, wait] 30.199878365s, 29.994999827s, 204.878538ms
Latencies [mean, 50, 95, 99, max] 89.449201ms, 19.427713ms, 379.14075ms, 433.422654ms, 455.042936ms
Bytes In [total, mean] 618000, 103.00
Bytes Out [total, mean] 0, 0.00
Success [ratio] 100.00%
Status Codes [code:count] 200:6000
```

The full code is resting [here](https://github.com/biancarosa/django-dashboard-api).

Overall impression: easy to setup, easy to use, nice docs, weird code (I’m sorry, I like my code cleaner, too many libs and words).