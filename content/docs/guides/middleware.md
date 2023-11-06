---
title: "Middleware"
description: ""
summary: ""
date: 2023-10-02T20:09:26+05:30
lastmod: 2023-10-02T20:09:26+05:30
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "middleware-9c2d9f464c200d13026f200460da8b66"
weight: 999
toc: true
---

> NOTE: at this time ovenjoy doesn't support all expressjs middleware

Middleware functions are an integral part of OvenJoy and are used to process requests and responses before they reach their final handler. Middleware functions can perform various tasks, such as modifying the request or response, authenticating users, logging, and more.

Middleware functions can perform the following tasks:

- Execute any code.
- Make changes to the request and the response objects.
- End the request-~~response~~ cycle.
- Call the next middleware function in the stack.

### Adding Middleware

To add middleware to a specific route, you can use the app.use() or app.get(), app.post(), etc., methods. Middleware functions are executed in the order they are added, and each middleware function must call next() to pass control to the next middleware or route handler.

Here's an example of adding middleware to a route:

```javascript
const queryMiddleware = (req, res, next) => {
  // This middleware parses the query string in the URL.
  req.querystring = function () {
    return require("url").parse(this.url).query;
  };
  next(); // Call next to pass control to the next middleware or route handler.
};

app.get("/foo", queryMiddleware, (req, res) => {
  // This route handler uses the data processed by the middleware.
  res.send(req.querystring());
});
```

**In the example above:**

- We define a queryMiddleware function that parses the query string in the URL.
- We use `app.get("/foo", queryMiddleware, ...)` to attach the middleware to the `/foo` route.
- Inside the middleware, we call `next()` to ensure that control is passed to the next middleware or the final route handler.

### Middleware Order

Middleware functions are executed in the order they are added to the route. You can control the order in which middleware functions are executed by arranging them accordingly.

---

OvenJoy application can use the following types of middleware:

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

### Application-level middleware

Bind application-level middleware to an instance of the app object by using the app.use() and app.METHOD() functions, where METHOD is the HTTP method of the request that the middleware function handles (such as GET, PUT, or POST) in lowercase.

This example shows a middleware function with no mount path. The function is executed every time the app receives a request.

```javascript
const OvenJoy = require("ovenjoy");
const app = OvenJoy();

app.use((req, res, next) => {
  console.log("Time:", Date.now());
  next();
});
```

> NOTE: at this time ovenjoy doesn't support path mounted middleware using `app.use(path, middleware)`

This example shows an array with a middleware sub-stack that handles GET requests to the /user/:id path

```javascript
function logOriginalUrl(req, res, next) {
  console.log("Request URL:", req.originalUrl);
  next();
}

function logMethod(req, res, next) {
  console.log("Request Type:", req.method);
  next();
}

const logStuff = [logOriginalUrl, logMethod];
app.get("/user/:id", logStuff, (req, res, next) => {
  res.send("User Info");
});
```

### Router-level middleware

Router-level middleware works in the same way as application-level middleware, except it is bound to an instance of `ovenjoy.Router()`.

```javascript
const OvenJoy = require("ovenjoy");
const app = OvenJoy();
const router = OvenJoy.Router();

// a middleware function with no mount path. This code is executed for every request to the router
router.use((req, res, next) => {
  console.log("Time:", Date.now());
  next();
});

// handler for the /user/:id path, which send a perticular user data
router.get("/user/:id", (req, res, next) => {
  console.log(req.params.id);
  res.json({ id: req.params.id });
});

// mount the router on the app
app.use("/", router);
```

### Error-handling middleware

TODO

### Third-party middleware

TODO: Third-party middleware support
