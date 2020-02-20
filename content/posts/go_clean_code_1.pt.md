+++
title = "Código Limpo em Go - Nomes de variáveis curtos são uma boa prática?"
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

Oi.

Falei um pouco sobre Go e Clean Code em alguns meetups e na última Gophercon Brasil em 2019 (slides [aqui](https://slides.com/biancarosa__/go-clean-code)) 
e agora estou quebrando essa apresentação em uma série de posts, começando com esse, para explicar que tipo de código é considerado *limpo* em Go. A motivação é dividir um pouco após anos de escrita de código em Go sobre as práticas consideradas boas / ruins.

Minha opinião modesta é que codigo limpo é *mais* do que uma série de Design Patterns ou princípios SOLID. Focamos muito nessas coisas e às vezes esquecemos em como escrever código que é facil de ser compreendido por outras pessoas que sabem programar nessa linguagem. Então eu sempre começo pelo *styleguide* da linguagem.

Eu tive muita dificuldade quando descobri que [nomes de variáveis em Go devem ser pequenos ao invés de grandes](https://github.com/golang/go/wiki/CodeReviewComments#variable-names). Reclamei um pouco com meus amiguinhos de trabalho, disse por aí que era um absurdo e muito estúpido. Eu estava sofrendo pelo fato de que sempre, sempre, desde que eu tinha lá meus quinze anos de idade e estava aprendendo a escrever código, meus professores me falaram pra escrever nomes que *façam sentido* e sejam *descritíveis*. Exemplos sempre saem de um livro de Java com um nome bastante descritivo para um método. 

Quando você vê [as notas de Rob Rike sobre seu estilo de programação](https://www.lysator.liu.se/c/pikestyle.html), ele pontua que: "Tamanho não é virtude de um nome; clareza de expressão *é*." (na seção **Variable names**).

Demorei um tempo pra perceber que nomes que *fazem sentido* e são *descritíveis* podem ser *pequenos*. E às vezes, quando você combina essas coisas seu código sai melhor.

Fazemos isso em outras linguagens mais do que imaginamos.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
for i := 0; i < 10; i++ {
    sum += i
}
{{< / highlight >}}

Nesse exemplo em particular, *i* é um nome bom para a variável *index*, enquanto *sum* na realidade é pequeno o suficiente para que não vire *s*.

Quando os **Code Review comments** de Go falam que você deve preferir *c* a *lineCount*, significa que esse código...

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

...*não* é mais legível do que este...

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

Por quê?

No dois casos, *i* e *c* são variáveis que *vivem pouco*. Seu escopo é pequenininho, limitado, o bloco de código em que são usadas é curto. Você vê aonde termina. Você não precisa descer muito a sua tela pra ler.

Numa regra mais geral, suas variáveis devem ter nomes que vão crescer de acordo com o *escopo* utilizado por elas. Não dê a louca e use *c* ao invés de *lineCount* em uma variável de pacote.

E *fuja de escopos grandes* - mas é um assunto pra um próximo post.

Ate mais :)

# Links de referência

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