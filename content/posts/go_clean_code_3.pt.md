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

Aqui eu quero explorar mais a arte não-tão popular de nomear funções e interfaces. Vou introduzir esses dois assuntos brevemente, falar sobre convenções de nomes para eles, e então prover alguns examplos com algumas explicações. E sempre com os links para as minhas referências :) 

A gente vai usar sempre **mixedCaps** ou **MixedCaps**, dependendo se queremos [exportar](https://tour.golang.org/basics/3) uma função/interface ou não.

A regra básica de nomear coisas deve ser aplicada aqui: seja conciso, descritivo, escolha nomes *pequenos e que representem algum significado*.
# Funções

Funções em Go são como funções na maioria das linguagens de programação. Elas realizam uma ação com base em um número específico de argumentos e podem ou não retornar valores. Elas podem ser [funções simples](https://gobyexample.com/functions), ter [múltiplos valores de retorno](https://gobyexample.com/multiple-return-values), podem ser [funções variadicas](https://gobyexample.com/variadic-functions), [funções recursivas](https://gobyexample.com/recursion) ou até mesmo [funções anônimas, também conhecidas como closures](https://gobyexample.com/closures).

## Getters e Setters

É muito comum, em várias linguagens, escrever getters e setters para propriedades que não são exportadas ou que requerem um tratamento especial.

Em Go, se quisermos **obter** a propriedade *owner* de um objeto, é assim que queremos fazer:

```go
obj.Owner()
```

Mas se quisermos **definir** a propriedade *owner*, queremos fazer:

```go
obj.SetOwner(owner)
```

A notícia mais surpreendente sobre essa abordagem é a falta da palavra *get* no **getter**. Seu uso é desencorajado por [Effective Go](https://golang.org/doc/effective_go.html#Getters).

# Interfaces

Um tipo de Interface mantém um conjunto de assinaturas de métodos. Go tem algo muito legal em que, para uma coisa ser do mesmo tipo de uma Interface, ela não precisa se referenciar explicitamente a ela - seguir o mesmo conjunto de assinaturas de métodos é suficiente!

Frequentemente, queremos manter nossas interfaces pequenas e com um propósito muito claro. Certo? Estou julgando você se fizer suas interfaces fazerem mais do que uma coisa, mas não se engane, faço isso mais vezes do que gostaria de admitir.

Quando temos interfaces com apenas um método, adotamos, por convenção, o nome do método + o sufixo -er (em inglês) ou -or (em português). Os seguintes nomes de interfaces são bem legais:

- Reader (Leitor)
- Writer (Escritor)
- Formatter (Formatador)
- Notifier (Notificador)

Normalmente, elas se parecem com isso:

```go
type Caller interface {
	Call()
}

type Notifier interface {
	Notify()
}
```

Em português, é mais difícil seguir essa regra - é possível tentar, mas nem sempre vai funcionar bem - mas a maioria das pessoas opta por escrever seu código em inglês.

["Sometimes the result isn't correct English, but we do it anyway. Sometimes we use English to make it nicer." -  Andrew Gerrand, What's in a name?](https://talks.golang.org/2014/names.slide#13)

# Links de referência

- https://talks.golang.org/2014/names.slide#13
- https://golang.org/doc/effective_go.html#Getters
- https://golang.org/doc/effective_go.html#mixed-caps
- https://github.com/golang/go/wiki/CodeReviewComments#interfaces
- https://medium.com/better-programming/naming-conventions-in-go-short-but-descriptive-1fa7c6d2f32a

# Posts Anteriores

- [Código Limpo em Go - Escrevendo bons nomes de pacote](../go_clean_code_2/)
- [Código Limpo em Go - Nomes de variáveis curtos são uma boa prática?](../go_clean_code_1/)