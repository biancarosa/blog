+++
title = "Clean Code in Go - Are short variable names a good practice?"
tags = [
    "go",
    "development",
    "clean code"
]
date = "2018-08-13"
categories = [
    "development"
]
author = "@__biancarosa"
+++

Hi.

I spoke about Go and Clean Code in a couple of meetups and at the last Gophercon Brasil in 2019 (slides [here](https://slides.com/biancarosa__/go-clean-code), in Portuguese) and I am now breaking the presentation in a series of posts, starting with this one, to explain what kind code is considered *clean* in Go. The motivation behind this is to share a little after years of writing Go code on what kind of practices are considered good / bad.

My very humble opinion is that a clean code is *more* than just a series of Design Patterns or SOLID principles. We often focus on these things and we forget about what's like to write a code that is easy to understand for other programmers in that particular programming language. So I always start with the language styleguide.

I struggled a lot when I found out that [Variable names in Go should be short rather than long](https://github.com/golang/go/wiki/CodeReviewComments#variable-names). I cried a little with my teammates, saying that it was just plain stupid. I was struggling with the fact that always, ever since when I was a fifteen year old kid learning how to code, my teachers told me to write *meaningful* and *descriptive* variable names. Examples were always from a Java code and a *very descriptive* method name.

When you see [Rob Pike's notes on his programming style](https://www.lysator.liu.se/c/pikestyle.html), he very well points that: "Length is not a virtue in a name; clarity of expression *is*." (under the section **Variable names**).

It took me a while that *meaningful* and *descriptive* can be *short*. And sometimes, when you combine these things, you end up with a better code.

And we do this more often in other languages than we believe.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
for i := 0; i < 10; i++ {
    sum += i
}
{{< / highlight >}}

In this particular example, *i* is a perfectly good name for the *index* variables, while *sum* is actually short enough so it doesn't need to become *s*.

When Go's Code Review comments says that you should prefer *c* to *lineCount*, it means that this code...

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
func countLines() int {
    // do stuff
    var linesCount int
    for i := 0; i < lines; i++ {
        linesCount += 1
    }
    return linesCount
}
{{< / highlight >}}

...is *not* more readable than this code...

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
func countLines() int {
    // do stuff
    var c int
    for i := 0; i < lines; i++ {
        c += 1
    }
    return c
}
{{< / highlight >}}

Why is that so?

In both cases, *i* and *c* are *short-lived variables*. Their scope is tiny, the code block that its used is *tiny*. You can see right where it ends. You don't have to scroll down too much.

On a more general rule, your variables names should grow according to its *scope*. Don't go all crazy and use *c* instead of *lineCount* for package variables.

And *avoid big scopes* - but this is a subject for our next post. See ya! :)

# Cool reference links

- https://research.swtch.com/names
- https://talks.golang.org/2014/names.slide#1
- http://wordaligned.org/articles/go-for-short-variable-names
- https://groups.google.com/forum/#!topic/golang-nuts/J9QeizedpuI
- https://softwareengineering.stackexchange.com/questions/176582/is-there-an-excuse-for-short-variable-names
- https://www.quora.com/Why-does-Golang-promote-short-and-kind-of-meaningless-names-for-variables
- https://blog.learngoprogramming.com/golang-short-variable-declaration-rules-6df88c881ee
- https://www.reddit.com/r/golang/comments/2hwch1/what_is_with_the_fascination_for_short_variable/
- https://www.lysator.liu.se/c/pikestyle.html
- https://github.com/golang/go/wiki/CodeReviewComments