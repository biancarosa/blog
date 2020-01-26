+++
title = "Writing distributed applications with Python Smart Objects as a Java RMI alternative"
tags = [
    "python",
    "development",
    "distributed-systems"
]
date = "2018-08-13"
categories = [
    "development"
]
author = "@__biancarosa"
+++

Hello there! :)

So, for those of you who want to write distributed systems easily in Python or are just curious about the subject, I was studying about [Java RMI](https://docs.oracle.com/javase/tutorial/rmi/) for my Distributed Systems class and I always get <sarcasm>thrilled</sarcasm>to know that I need to write Java code. Nothing against good ol’ Java, it’s just not my vibe these days but I’d happily do it if convinced. Since I was just studying and trying to learn a concept, I searched for a Python alternative.

And I found it.

Python Smart Objects are implemented in this library named [Pyro]
(http://pyro4.readthedocs.io/en/stable/) and it amazes me how simple and easy it is to write a distributed application with it. It works on multiple Python versions, it is being actively updated by its author and its seamlessly to use.

I’ve made a small chat distributed system named [mess](https://github.com/biancarosa/mess) that makes use of Pyro, but I’ll show a simpler version of the code here.

This is the code for the client. It creates a proxy to the server and sends a text message.

<script src="https://gist.github.com/biancarosa/ff552e5d7244112e9e71267b5211be6d.js"></script>

And this is the code for the server. It locates a name server (Pyro comes with one and you just need to use it with `python -m Pyro4.naming` in the same machine as the server).

<script src="https://gist.github.com/biancarosa/26f52dbbeadfa63d725d561eafa52a0e.js"></script>

And that’s all :) You can run multiple clients, but only one server per name, so if you need multiple servers for some reason you’re gonna need to register names with a identifier.

Disclaimer: I never needed to use [Python Smart Objects](http://pyro4.readthedocs.io/) neither [Java RMI](https://docs.oracle.com/javase/tutorial/rmi/) in a real world application. I came across RMI in a college class about Distributed Systems. It’s a nice thing to study as an alternative because it is the kind of knowledge that you might need in the future. Nowadays, if I really need distributed systems with high performance, I’d try [GRPC](https://grpc.io/), probably with [Go](https://golang.org/).