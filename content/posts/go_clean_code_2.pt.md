+++
title = "Código Limpo em Go - Escrevendo bons nomes de pacote"
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
+++

Esse é o segundo post de uma série de posts que começou [aqui](https://biancarosa.com.br/posts/go_clean_code_1/) baseado numa palestra dada sobre Go e Clean Code(slides [aqui](https://slides.com/biancarosa__/go-clean-code))

Agora vou falar sobre nomes de pacotes e como escolher nomes de pacotes em Go.

Primeiro de tudo, **pacotes** em Go é como o código Go é organizado. É similar a um **módulo** Python e basicamente o mesmo do que um **pacote** Java, um **namespace** em C# ou a um **componente** JavaScript. É uma *pasta* contendo código que, numa boa arquitetura de software, compartilham coisas em comum.

Os guidelines dados pelo [Effective Go](https://golang.org/doc/effective_go.html#names) sobre nomear pacotes são bem bons - no fim das contas, o principal é que você deve:

1. Mantê-los pequenos e concisos (e não se preocupar com colisões de nomes).
2. Escolher um nome que é bom o suficiente pra dar contexto para suas Interfaces, Structs e Functions exportadas (portanto, públicas).

O primeiro ponto é focado na facilidade de leitura do nome do seu pacote. Colisões de nome raramente acontecem e quando acontecer, quem está importando o pacote pode tomar uma decisão de alterar a referência do pacote importado, como no exemplo abaixo. Não é responsabilidade de um pacote evitar colisões de nome, e sim de quem importa ele.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
import mylogger "log"
{{< / highlight >}}

O segundo ponto fica mais claro quando você vê a implementação da interface Reader em vários pacotes:

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
import "io"
import "bufio"
import "bytes"

var reader *io.Reader
var bufreader *bufio.Reader
var bytesreader *bytes.Reader
{{< / highlight >}}

Em todos os três casos, o tipo é Reader e o nome do pacote é pequeno, conciso o suficiente e dá contexto sobre o conteúdo do pacote.

Quando a biblioteca do Go ganhou uma implementação de uma lista circular (ou anel), ela ganhou um pacote chamado *ring*. Inicializar uma nova lista circular é apenas chamar *ring.New()*. Normalmente, constructors em Go tem essa cara.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
import "ring"
ring := ring.New()
{{< / highlight >}}

Bons nomes de pacotes são *muito* importantes para escrever um código limpo e fácil de ler. [Esse post](https://blog.golang.org/package-names) sobre nomes de pacotes em Go também é essencial.

É basicamente isso que eu queria dividir :) Sugiro ler as referências, também!

# Cool reference links

- https://blog.golang.org/package-names
- https://golang.org/doc/effective_go.html#names
- https://rakyll.org/style-packages/

# Past Posts

- [Clean Code in Go - Are short variable names a good practice?](../go_clean_code_1/)