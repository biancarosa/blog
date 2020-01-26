+++
title = "Why you should choose your Python module names more carefully"
description = ""
tags = [
    "python",
    "development",
    "en"
]
date = "2017-08-30"
categories = [
    "development",
    "en"
]
highlight = "true"
+++

We code everyday. We produce code fast, following the rhythm of the rock and roll guitar playing in the background and every time we focus on writing code fast, we miss some important things that makes us lose time in the future.

The next time you create a new file in your Python project, think about its name very carefully. Every Python file is a module. We import modules all the time. If I create a file named after an already existent module — and worst, already imported module, I am going to spend at least ten minutes trying to figure out what the heck is wrong with my code. Just because I chose a bad file name. Some of them are obvious and we should definitely avoid.

This is a file called numpy.py:

<script src="https://gist.github.com/biancarosa/720db847d568caba3a4a238ff54391b2.js"></script>

Or worst, if you have a file named numpy.py and the same method as the actual numpy module, you might think you’re calling a function but you’re actually calling another…

<script src="https://gist.github.com/biancarosa/8093788ce5dfc8f3c992c43c44b80028.js"></script>

<script src="https://gist.github.com/biancarosa/0656d554dc9a969c47511c09e392e7ef.js"></script>

This is also a problem when you import everything from a module. You don’t know what you are importing. Maybe the module you are importing has the same function as one you already defined. You have to dig into the module’s code to find out.

Cool tips:

- Avoid using import *.
- Avoid using common names for modules.
- Think about what you’re doing instead of typing code as fast as your fingers allow you.
- Naming things is important. Name them better.

PS: Stuff like this happens in real life, it happened once to colleagues of mine at work and it happened with me today. With that same example I shared in this post. Because I was sleepy and numpy.py seemed like a good filename to play with numpy. The only reason why I was able to find out what was happening fast was because I had seen it happening before.