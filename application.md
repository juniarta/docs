---
description: The app instance conventionally denotes the Fiber application.
---

# 🚀  Application

## New

This method creates a new **Fiber** named instance. You can pass optional [settings ](application.md#settings)when creating a new instance.

**Signature**

```go
fiber.New(settings ...*Settings)
```

**Example**

```go
package main

import "github.com/gofiber/fiber"

func main() {
    app := fiber.New()

    // Your app logic here ...

    app.Listen(3000)
}
```

## Settings

You can pass application settings when calling `New`, or change the settings before you call `Listen`

**Example**

```go
func main() {
    // Pass Settings creating a new app
    app := fiber.New(&fiber.Settings{
        Prefork:       true,
        CaseSensitive: true,
        StrictRouting: true,
        ServerHeader:  "Fiber",
        // ...
    })

    // Or change Settings after initiating app
    app.Settings.Prefork = true
    app.Settings.CaseSensitive = true
    app.Settings.StrictRouting = true
    app.Settings.ServerHeader = "Fiber"
    // ...

    app.Listen(3000)
}
```

**Common options**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Prefork</td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left">Enables use of the<a href="https://lwn.net/Articles/542629/"><code>SO_REUSEPORT</code></a>socket
        option. This will spawn multiple Go processes listening on the same port.
        learn more about <a href="https://www.nginx.com/blog/socket-sharding-nginx-release-1-9-1/">socket sharding</a>.</td>
      <td
      style="text-align:left"><code>false</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">ServerHeader</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Enables the <code>Server</code> HTTP header with the given value.</td>
      <td
      style="text-align:left"><code>&quot;&quot;</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">StrictRouting</td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left">When enabled, the router treats <code>/foo</code> and <code>/foo/</code> as
        different. Otherwise, the router treats <code>/foo</code> and <code>/foo/</code> as
        the same.</td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">CaseSensitive</td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left">When enabled, <code>/Foo</code> and <code>/foo</code> are different routes.
        When disabled, <code>/Foo</code>and <code>/foo</code> are treated the same.</td>
      <td
      style="text-align:left"><code>false</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Immutable</td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left">When enabled, all values returned by context methods are immutable. By
        default they are valid until you return from the handler, see issue <a href="https://github.com/gofiber/fiber/issues/185">#185</a>.</td>
      <td
      style="text-align:left"><code>false</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Compression</td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left">Enables GZip / Deflate compression for all responses.</td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BodyLimit</td>
      <td style="text-align:left"><code>int</code>
      </td>
      <td style="text-align:left">Sets the maximum allowed size for a request body, if the size exceeds
        the configured limit, it sends <code>413 - Request Entity Too Large</code> response.</td>
      <td
      style="text-align:left"><code>4 * 1024 * 1024</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">TemplateFolder</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>A directory for the application&apos;s views. If a directory is set, this
          will be the prefix for all template paths.</p>
        <p><code>c.Render(&quot;home.pug&quot;, d) -&gt; /views/home.pug</code>
        </p>
      </td>
      <td style="text-align:left"><code>&quot;&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">TemplateEngine</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The template engine to use: <code>html</code>, <a href="https://github.com/eknkc/amber"><code>amber</code></a>,
        <a
        href="ttps://github.com/aymerick/raymond"><code>handlebars</code>
          </a>, <code>mustache</code> or <a href="https://github.com/Joker/jade"><code>pug</code></a>.</td>
      <td
      style="text-align:left"><code>&quot;&quot;</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">TemplateExtension</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">If you preset the template file extension, you do not need to provide
        the full filename in the Render function: <code>c.Render(&quot;home&quot;, data) -&gt; home.pug</code>
      </td>
      <td style="text-align:left"><code>&quot;html&quot;</code>
      </td>
    </tr>
  </tbody>
</table>## Static

Serve static files such as **images**, **CSS** and **JavaScript** files, you can use the **Static** method.

{% hint style="info" %}
By default, this method will send `index.html` files in response to a request on a directory.
{% endhint %}

**Signature**

```go
app.Static(prefix, root string) // => with prefix
```

**Examples**

Use the following code to serve files in a directory named `./public`

```go
app.Static("/", "./public")

// => http://localhost:3000/hello.html
// => http://localhost:3000/js/jquery.js
// => http://localhost:3000/css/style.css
```

To serve from multiple directories, you can use **Static** multiple times.

```go
// Serve files from "./public" directory:
app.Static("/", "./public")

// Serve files from "./files" directory:
app.Static("/", "./files")
```

{% hint style="info" %}
Use a reverse proxy cache like [NGINX](https://www.nginx.com/resources/wiki/start/topics/examples/reverseproxycachingexample/) to improve performance of serving static assets.
{% endhint %}

You can use any virtual path prefix \(_where the path does not actually exist in the file system_\) for files that are served by the **Static** method, specify a prefix path for the static directory, as shown below:

```go
app.Static("/static", "./public")

// => http://localhost:3000/static/hello.html
// => http://localhost:3000/static/js/jquery.js
// => http://localhost:3000/static/css/style.css
```

## HTTP Methods

Routes an HTTP request, where **METHOD** is the [HTTP method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) of the request.

**Signature**

```go
// HTTP methods support :param, :optional? and *wildcards
// You are required to pass a path to each method
app.All(path string, handlers ...func(*Ctx))
app.Get(...
app.Put(...
app.Post(...
app.Head(...
app.Patch(...
app.Trace(...
app.Delete(...
app.Connect(...
app.Options(...

// Use() will only match the beggining of each path
// i.e. "/john" will match "/john/doe", "/johnnnn"
// Use() does not support :param & :optional? in path
app.Use(handlers ...func(*Ctx))
app.Use(prefix string, handlers ...func(*Ctx))
```

**Example**

```go
app.Use("/api", func(c *fiber.Ctx) {
  c.Set("X-Custom-Header", random.String(32))
  c.Next()
})
app.Get("/api/list", func(c *fiber.Ctx) {
  c.Send("I'm a GET request!")
})
app.Post("/api/register", func(c *fiber.Ctx) {
  c.Send("I'm a POST request!")
})
```

## WebSocket

Fiber supports a websocket upgrade implementation for fasthttp. The `*Conn` struct has all the functionality from the [gorilla/websocket ](https://pkg.go.dev/github.com/fasthttp/websocket?tab=doc#pkg-index)library.

**Signature**

```go
app.WebSocket(path string, handler func(*Conn))
```

{% hint style="warning" %}
WebSocket does not support path parameters and wildcards.
{% endhint %}

**Example**

```go
package main

import (
    "log"
    "github.com/gofiber/fiber"
)

func main() {
    app := fiber.New()
    // Optional middleware
    app.Use("/ws", func(c *fiber.Ctx) {
        if c.Get("host") != "localhost:3000" {
            c.Status(403).Send("Request origin not allowed")
        } else {
            c.Next()
        }
    })
    // Upgraded websocket request
    app.WebSocket("/ws", func(c *fiber.Conn) {
        for {
            mt, msg, err := c.ReadMessage()
            if err != nil {
                log.Println("read:", err)
                break
            }
            log.Printf("recv: %s", msg)
            err = c.WriteMessage(mt, msg)
            if err != nil {
                log.Println("write:", err)
                break
            }
        }
    })
  // ws://localhost:3000/ws
    app.Listen(3000)
}
```

## Group

You can group routes by creating a `*Group` struct.

**Signature**

```go
app.Group(prefix string, handlers ...func(*Ctx)) *Group
```

**Example**

```go
func main() {
  app := fiber.New()

  api := app.Group("/api", cors())  // /api

  v1 := api.Group("/v1", mysql())   // /api/v1
  v1.Get("/list", handler)          // /api/v1/list
  v1.Get("/user", handler)          // /api/v1/user

  v2 := api.Group("/v2", mongodb()) // /api/v2
  v2.Get("/list", handler)          // /api/v2/list
  v2.Get("/user", handler)          // /api/v2/user

  app.Listen(3000)
}
```

## Listen

Binds and listens for connections on the specified address. This can be a `int` for port or `string` for address.

**Signature**

```go
app.Listen(address interface{}, tls ...*tls.Config)
```

**Example**

```go
app.Listen(8080)
app.Listen("8080")
app.Listen(":8080")
app.Listen("127.0.0.1:8080")
```

To enable **TLS/HTTPS** you can append a ****[**TLS config**](https://golang.org/pkg/crypto/tls/#Config).

```go
cer, err := tls.LoadX509KeyPair("server.crt", "server.key")
if err != nil {
    log.Fatal(err)
}
config := &tls.Config{Certificates: []tls.Certificate{cer}}

app.Listen(443, config)
```

## Test

Testing your application is done with the **Test** method.

{% hint style="info" %}
Method is mostly used for `_test.go` files and application debugging.
{% endhint %}

**Signature**

```go
app.Test(req *http.Request) (*http.Response, error)
```

**Example**

```go
// Create route with GET method for test:
app.Get("/", func(c *Ctx) {
  fmt.Println(c.BaseURL())              // => http://google.com
  fmt.Println(c.Get("X-Custom-Header")) // => hi

  c.Send("hello, World!")
})

// http.Request
req, _ := http.NewRequest("GET", "http://google.com", nil)
req.Header.Set("X-Custom-Header", "hi")

// http.Response
resp, _ := app.Test(req)

// Do something with results:
if resp.StatusCode == 200 {
  body, _ := ioutil.ReadAll(resp.Body)
  fmt.Println(string(body)) // => Hello, World!
}
```

