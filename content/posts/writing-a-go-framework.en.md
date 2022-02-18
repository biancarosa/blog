+++
title = "i'm writing a go framework!"
description = ""
tags = [
    "go",
    "development",
	"web",
	"api",
	"framework",
	"backend"
]
date = "2022-02-16"
categories = [
    "development"
]
author = "@__biancarosa"
+++

Hello folks!

While working at [Red Hat](https://www.redhat.com) I had the awesome opportunity to brush up my Go skills on a daily basis again, and I stumbled across a lot of interesting technical problems.

One of these problems struck my attention so much that I started playing with it on my free time and I came up with **[ez](https://github.com/biancarosa/ez)**. This is a pet project, but I wanted to let people know I'm on it in case its going to be useful for someone else and/or if you wanna get involved and contribute!

My main motivator is that I like writing code for web APIs but I want API docs to be automatically generated for them. I do not want my docs to generate code - quite the opposite -, and most of the current robust solutions for Go provide that but I haven't found one that generates docs from the code properly. Feel free to comment, or reach out to me if you know though! Always happy to know I did a bad search.

To solve that problem, **[ez](https://github.com/biancarosa/ez)** (comes from easy. yes it's cheesy.) is on the way. So far, the use-case scenario that I have for **[ez](https://github.com/biancarosa/ez)** is...
{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
func main() {
	server := ez.New(&http.Server{
		Addr: fmt.Sprintf(":%s", port),
	})
	server.RegisterRoute(ez.Route{
		Handler:  RouteHandler,
		Pattern:  "/",
		Method:   []string{http.MethodGet},
		Response: Response{},
	})

	server.GenerateDocs()

	server.ListenAndServe()
}
{{< / highlight >}}

Where RouteHandler is handler function, the Pattern is the URL pattern, the method the accepted methods of the route and the response object is provided. With that, the `server.GenerateDocs()` method can generate a very-simple openapi doc for a GET method:

{{< highlight yaml "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
components:
  schemas:
    Response:
      properties:
        message:
          type: string
      type: object
paths:
  /:
    get:
      operationId: operation
      responses: {}
{{< / highlight >}}

I wanna update for other kinds of methods and take it more seriously, but if you think that's a good idea, at least give it a star on github as an incentive! And if you wanna contribute, just reach out to me and lets do this!