+++
title = "Código Limpo em Go - Nomeando Funções e Interfaces"
tags = [
    "go",
    "development",
    "clean code"
]
date = "2020-03-15"
categories = [
    "development"
]
author = "@__biancarosa"
draft = true
+++

DISCLAIMER: Esses posts não tem **nada** a ver com um certo livro que fala sobre Código Limpo. Nada. Aqui eu tomo a liberdade de definir o que código limpo, especialmente em Go, baseado na minha própria experiência em escrever muito código uhm, não limpo ao longo dos anos, várias pesquisas que faço pra tornar meu código melhor *enquanto estou escrevendo em Go*. Esses exemplos **provavelmente não se aplicarão diretamente a nenhuma outra linguagem de programação.*.

Esse é o segundo post de uma série de posts que começou [aqui](hhttps://biancarosa.com.br/pt/posts/go_clean_code_1/) baseado numa palestra dada sobre Go e Clean Code(slides [aqui](https://slides.com/bianca_rosa/go-clean-code))

I'd like to explore a bit more on the unpopular art of naming functions and interfaces. I am gonna introduce these things very quickly, then I'm gonna talk about a couple of naming conventions for then, and provide a couple of examples alongside with a brief explanation of these examples. As usual, I will provide links to my references.

We'll always use **mixedCaps** or **MixedCaps**, depending on whether we want to [export](https://tour.golang.org/basics/3) a name or not.

The basic rules of naming things should be applied here: be concise, be descriptive. Pick *short and meaningful names*.

# Functions

Functions in Go are like functions in pretty much every programming language. They do something given a specific numbers of arguments and may or may not return values. They can be [simple functions](https://gobyexample.com/functions), have [multiple return values](https://gobyexample.com/multiple-return-values), they can be [variadic function](https://gobyexample.com/variadic-functions), [recursive functions](https://gobyexample.com/recursion) or even [anonymous functions also known as closures](https://gobyexample.com/closures).

## Getters and Setters

It is very common, in several languages, to write getters and setters for properties that are not exported or that require a special treatment. 

In Go, if we want to **get** the *owner* property of an object, this is how we want to do it:

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
obj.Owner()
{{< / highlight >}}

But if we want to **set** the *owner* property, we want to do:

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
obj.SetOwner(owner)
{{< / highlight >}}

The most shockingly news about this approach is the lack of the word *get* on the **getter**. Its use is discouraged by [Effective Go](https://golang.org/doc/effective_go.html#Getters)

# Interfaces

An Interface type holds a set of method signatures. Go has thing very cool thing that for a thing to be of the same type of an Interface, it doesn't have to explicitly reference it - following the same set of method signatures is enough!

We often want to keep our interfaces tiny and with a very clear purpose. Right? I'm judging you if you make your interfaces do more than one thing, but don't be mistaken, I do it more often than I'd like to admit.

When we have interfaces that have only one method, we adopt, by convention, the name of the method + the suffix -er. The following interface names are pretty dope:

- Reader
- Writer
- Formatter
- Notifier

They normally look like that:
{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
type Caller interface {
	Call()
}

type Notifier interface {
	Notify()
}{{< / highlight >}}

["Sometimes the result isn't correct English, but we do it anyway. Sometimes we use English to make it nicer." -  Andrew Gerrand, What's in a name?](https://talks.golang.org/2014/names.slide#13)

# Cool reference links

- https://talks.golang.org/2014/names.slide#13
- https://golang.org/doc/effective_go.html#Getters
- https://golang.org/doc/effective_go.html#mixed-caps
- https://github.com/golang/go/wiki/CodeReviewComments#interfaces
- https://medium.com/better-programming/naming-conventions-in-go-short-but-descriptive-1fa7c6d2f32a

# Past Posts

- [Clean Code in Go - Writing good package names](../go_clean_code_2/)
- [Clean Code in Go - Are short variable names a good practice?](../go_clean_code_1/)