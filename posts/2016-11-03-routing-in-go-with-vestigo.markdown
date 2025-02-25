---
date   2016-11-03 10:00:00
---

I was recently looking for an easy way to do routing in Go.

There are a few packages out there, but they all feel a little too heavy. I wanted something that just did simple routing, with little overhead.

I stumbled across <a href="https://github.com/husobee/vestigo" target="_blank">Vestigo</a>. It's really simple and looks a lot like the normal code you would write with Go's <code>net/http</code> package, only with some routing goodness on top.


# Using Vestigo
Firstly, grab the vestigo package:

```bash
go get github.com/husobee/vestigo
```

The Vestigo router implements <code>http.Handler</code>, so you can pass it straight into the <code>http.ListenAndServe</code> function.

This is all you need to get a web app going:


```go
package main

import (
    "net/http"
    "github.com/husobee/vestigo"
)

func main() {

    // Create the router
    router := vestigo.NewRouter()

    // Define a route.
    // This matches GET requests to routes such as '/home/dave'
    // and redirects them to the homeHandler() function
    router.Get("/home/:name", homeHandler)

    // Start the HTTP server, passing in the Vestigo router
    http.ListenAndServe(":1234", router)
}


func homeHandler(w http.ResponseWriter, r *http.Request) {

    // This gets the 'name' parameter from the route
    name := vestigo.Param(r, "name")

    w.Write([]byte("Hey there " + name))
}

```

That's all there is to it! 



# Further reading

If you want to find out more about Vestigo, check out these links:

<ul>
    <li>The project's <a href="https://github.com/husobee/vestigo" target="_blank">Github repo</a></li>
    <li>A <a href="https://husobee.github.io/golang/urlrouter/vestigo/2015/09/22/vesigo.html" target="_blank">blog post</a> by the author</li>
</ul>


