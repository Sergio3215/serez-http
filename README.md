# serez-http

HTTP and WebSocket server for [Serez-Code](https://serezcode.org) — an Express/Flask-style API
with routing, middleware, rate limiting and CORS, written in pure `.sz`. Build a REST API in a
few lines and run it with `sz`:

```serez
import "serez-http"

const app = new App()

app.GET("/", fn(req, res) {
    let r <string, any> = ({"message", "hello from serez-http"})
    res.json(r)
})

app.GET("/user/:id", fn(req, res) {
    let r <string, any> = ({"user_id", req["params"]["id"]})
    res.json(r)
})

app.POST("/echo", fn(req, res) {
    res.json(JSON.parse(req["body"]))
})

app.listen(3000, fn() {
    out "server running on port 3000"
})
```

## Install

```powershell
sz install serez-http
```

The library declares the `Socket` and `Time` permissions itself — your app's `serez.json` does
not need to add them.

## Quick use

- **Routes**: `app.GET / app.POST / app.PUT / app.delete(path, fn(req, res) {...})`. Patterns
  support `:param` segments, captured in `req["params"]`. (`get` and `use` are Serez-Code
  keywords — that's why the verbs are uppercase and middleware is `addMw`.)
- **Request**: `req` is a `<string, any>` dict — `req["method"]`, `req["path"]`, `req["query"]`,
  `req["params"]`, `req["body"]`, `req["headers"]`, `req["ip"]`, `req["authorization"]`, …
- **Response**: chainable — `res.status(404).json(obj)`, plus `send`, `header`, `cookie`,
  `redirect`, `sendfile`, `download`.
- **Middleware**: `app.addMw(fn(req, res, next) {...})` — call `next()` to continue the chain;
  global error handler via `app.error(fn(err, req, res) {...})`.
- **Security**: built-in `app.rateLimit(ip, max, seconds)` and a `corsMiddleware(origins,
  methods, headers)` factory.
- **WebSocket**: `app.ws(path, fn(req, ws) { ws.on("message", ...); ws.listen() })`.
- Single-threaded (one connection at a time) and bound to `127.0.0.1` — for deployment with
  external access, package it with [serez-apipack](https://serezcode.org/docs/serez-apipack).

## Documentation

- **[serez-http reference](https://serezcode.org/docs/serez-http)** — the request/response
  objects, middleware, security helpers and WebSocket, with full examples, on the Serez-Code site.
- **[Build a REST API](https://serezcode.org/guides/rest-api)** — step-by-step tutorial from
  `sz install` to a deployed, Dockerized API.
- **[Serve a model as an API](https://serezcode.org/guides/serve-model)** — serez-ai + serez-http
  together.
