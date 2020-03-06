+++
title = "Clean Code in Go - Writing good package names"
tags = [
    "go",
    "development",
    "clean code"
]
date = "2020-03-06"
categories = [
    "development"
]
author = "@__biancarosa"
draft = "true"
+++

This is a second post of a series that started [here](https://biancarosa.com.br/posts/go_clean_code_1/) based on a talk I gave about Go and Clean Code(slides [here](https://slides.com/biancarosa__/go-clean-code), in Portuguese)

Now I'm gonna talk about package names and how to choose package names in the Go way.

First of all, **packages** in Go is how Go code is organized. It is similar to a Python **module** and pretty much the same as a Java **package**, a C# **namespace** or a JavaScript **component**. It is a *folder* containing code that, in a good software architecture, has something in common.

The guidelines provided by [Effective Go](https://golang.org/doc/effective_go.html#names) on naming packages are pretty good - the bottom line is that you should:

1. Keep 'em short and concise (and don't worry about name collisions)
2. Choose a name good enough to give context to your exported Interfaces, Structs and Function.

The first one is for readability. Name collisions hardly happens, and when they do, the importer of the package can make a decision and change the reference of the imported package. It's not up to a package to avoid name collisions, but to its importers.

The second one is kinda tricky, but when you see the implementation of the Reader interface across packages, it's easier to see the point:

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
import "io"
import "bufio"
import "bytes"

var reader *io.Reader
var bufreader *bufio.Reader
var bytesreader *bytes.Reader
{{< / highlight >}}

In all of the three cases, the type is Reader and the package name is short, concise enough and it gives context to the content of the package.

When Go's standard library gained an implementation of a Ring data structure, it gained a package named *ring*. Initializing a new ring was just a matter of calling *ring.New()*.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
import "ring"
ring := ring.New()
{{< / highlight >}}

Good package names are *very* important to writing a code that's nice and easy to read. I also [this](https://blog.golang.org/package-names) blog post regarding package names!

So, that pretty much sums up what I what to say. I strongly recommend reading the references!

# Cool reference links

- https://blog.golang.org/package-names
- https://golang.org/doc/effective_go.html#names
- https://rakyll.org/style-packages/

# Past Posts

- [Clean Code in Go - Are short variable names a good practice?](../go_clean_code_1/)