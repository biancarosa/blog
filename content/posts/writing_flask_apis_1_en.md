+++
title = "A Comprehensive Guide To Write Flask APIs - #1"
description = ""
tags = [
    "python",
    "development",
    "en"
]
date = "2019-07-14"
categories = [
    "development",
    "en"
]
draft = "true"
highlight = "true"
+++

So, you met Flask. You fell in love and you started to to everything with Flask. It soon hits you that your code is a mess and it's either the framework's fault or your fault. Maybe you haven't realized it and the news come from a coworker. Or as soon as someone starts working with your code, the poor person will freak out.

You're not alone. Everyone writes shitty flask applications while learning. Unless you're some kind of rockstar developer or 10x engineer - in that case, my pardon for generalizing.

Writing Flask applications means having **freedom**. Freedom is not necessarily a good thing if you're a follower of a framework's patterns and just wants to do things fast. But the best thing about freedom is that it **forces** you to think.

Just thinking never took me to the next level, though. Searching for examples and watching how people do it are way more effective. Creating my own stuff based on the work of others speeds the learning curve in an amazing way.

This is not a tutorial for you to **follow**. This is a series of posts showing how **I** it. You'll adapt to your style and I expect you to contribute with how you do it.

First things first.

Create a directory with your app name.

So this is a good starting point for a Flask application:

```python
"""app.main

Module that starts the Flask application
"""
from flask import Flask, jsonify

app = Flask(**name**)

@app.route('/')
def healthcheck():
"""Returns health information"""
return jsonify({
"message": "I feel good."
})
```