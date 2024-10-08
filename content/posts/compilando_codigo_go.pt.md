---
title: "Compilando Código Go"
description: "O processo de compilação de um código Go"
tags: [
    "development",
    "go",
]
date: "2024-09-13"
categories: [
    "career"
]
author: "@__biancarosa"
---
# Compilando Código Go

Go oferece muitas vantagens para desenvolvedores, especialmente em termos de eficiência e portabilidade. 

Como Go é uma linguagem compilada, vale a pena falarmos um pouco sobre esse processo e o que ele significa. Além de como fazer, e mostrar alguns exemplos!

## Fundamentos

O processo de compilação segue esta sequência:

```
CÓDIGO -> COMPILADOR GO -> LINGUAGEM DE MÁQUINA -> LINKING -> EXECUTÁVEL
```

Comandos básicos de compilação:
- `go run main.go`: Compila e executa o arquivo.
- `go build`: Compila o código e gera um executável.

Uma característica notável do Go é a facilidade de compilação cruzada - isso é, para diferentes sistemas operacionais e arquiteturas a partir de uma única máquina.

Variáveis importantes:
- `GOOS`: Define o sistema operacional alvo (ex: linux, windows, darwin)
- `GOARCH`: Especifica a arquitetura alvo (ex: amd64, 386, arm)

Exemplo de comando:
```
GOOS=windows GOARCH=amd64 go build
```

Algumas outras vantagens que temos ao usar Go é que...

1. Os binários (executáveis gerados) são autocontidos: Incluem a maioria das dependências necessárias.
2. Compilação eficiente: Processo rápido, e o compilador é extremamente útil para entender erros no nosso programa.
3. Suporte multiplataforma: Facilita o desenvolvimento para diferentes sistemas operacionais.

## Aplicações Práticas

### 1. Distribuição de Aplicativos

Go permite compilar aplicativos para múltiplas plataformas facilmente:

```bash
GOOS=linux GOARCH=amd64 go build -o app-linux-amd64
GOOS=darwin GOARCH=arm64 go build -o app-mac-arm64
GOOS=windows GOARCH=amd64 go build -o app-windows-amd64.exe
```

### 2. Desenvolvimento de Microserviços

Exemplo de Dockerfile para criar containers leves:

```dockerfile
FROM golang:1.22 as builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:latest  
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```

### 3. Desenvolvimento Multiplataforma

Script para compilar para múltiplas plataformas:

```bash
#!/bin/bash
platforms=("windows/amd64" "darwin/amd64" "linux/amd64")

for platform in "${platforms[@]}"
do
    platform_split=(${platform//\// })
    GOOS=${platform_split[0]}
    GOARCH=${platform_split[1]}
    output_name='app-'$GOOS'-'$GOARCH
    if [ $GOOS = "windows" ]; then
        output_name+='.exe'
    fi

    env GOOS=$GOOS GOARCH=$GOARCH go build -o $output_name
    if [ $? -ne 0 ]; then
        echo 'Falha na compilação para '${GOOS}' '${GOARCH}
        exit 1
    fi
done
```

### 4. Desenvolvimento para IoT

Compilação para dispositivos embarcados:

```
GOOS=linux GOARCH=arm GOARM=7 go build -ldflags="-s -w" -o iot-app
```

### 5. Criação de Plugins

Compilação de plugins:

```
go build -buildmode=plugin -o myplugin.so myplugin.go
```
