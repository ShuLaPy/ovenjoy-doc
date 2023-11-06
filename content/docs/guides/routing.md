---
title: "Routing"
description: ""
summary: ""
date: 2023-11-06T01:23:50+05:30
lastmod: 2023-11-06T01:23:50+05:30
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "routing-ed9b3d7548e325d6c32094f4b8ef581a"
weight: 999
toc: true
---

_Routing_ determines how an application responds to a client request to a particular endpoint

I used Trie Tree for storing routing.

Route definition takes the following structure

```javascript
ovenjoy.METHOD(PATH, HANDLER);
```

Where:

- `METHOD` is an [HTTP request method](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods), in lowercase.
- `PATH` is a path on the server.
- `HANDLER` is the function executed when the route is matched.

### Routing methods

Ovenjoy supports the following routing methods corresponding to the HTTP methods of the same names:

|             |            |          |
| ----------- | ---------- | -------- |
| - get       | - post     | - put    |
| - delete    | - patch    | - option |
| - head      | - copy     | - lock   |
| - move      | - propfind | - unlock |
| - proppatch | - mkcol    | - trace  |
| - search    | - mount    |          |

You can define routes in following way.

```javascript
// Import the OvenJoy framework
import OvenJoy from "ovenjoy";

// Create an OvenJoy application
const app = OvenJoy();

// Get route
app.get("/users", (req, res) => {
  res.json({ users: ["user1", "user2", "user3"] });
});

// post route
app.post("/", (req, res) => {
  const users = ["user1", "user2", "user3"];
  const { user } = req.body;
  res.json({ users: [...users, user] });
});

// Start the server
app.listen(3000, () => {
  console.log("OvenJoy server is running on port 3000");
});
```

### Route Parameters

Route parameters are named segments in the URL that capture values at their position. These values populate the `req.params` object with the route parameter name as their keys.

```
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

```javascript
// Get route with params
app.get("/users/:userId/books/:bookId", (req, res) => {
  const { userId, bookId } = req.params;
  res.send("ok");
});
```

### Route Query

This property is an object containing a property for each query string parameter in the route.

```
Route path: /users
Request URL: http://localhost:3000/users?page=1&limit=100
req.query: { "page": "1", "limit": "100" }
```

```javascript
// Get route query for
// http://localhost:3000/users?page=1&limit=100
app.get("/users", (req, res) => {
  const { page, limit } = req.query;
  res.send("ok");
});
```

### Route handlers

You can provide multiple callback functions that behave like middleware to handle a request. You can use this mechanism to impose pre-conditions on a route, then pass control to subsequent routes if there’s no reason to proceed with the current route.

A single callback function can handle a route. For example:

```javascript
app.get("/example/a", (req, res) => {
  res.send("Hello from A!");
});
```

More than one callback function can handle a route (make sure you specify the next object). For example:

```javascript
app.get(
  "/example/b",
  (req, res, next) => {
    console.log("the response will be sent by the next function ...");
    next();
  },
  (req, res) => {
    res.send("Hello from B!");
  }
);
```

An array of callback functions can handle a route. For example:

```javascript
const cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

const cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

const cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])
```

### Sub Routes

You can create New Routers and Mount them on the Main Router to act as Sub Routers.

The following example creates a router as a module, loads a middleware function in it, defines some routes, and mounts the router module on a path in the main app.

Create a router file named birds.js in the app directory, with the following content:

For Example:

```javascript
const OvenJoy = require("ovenjoy");
const router = OvenJoy.Router();

// middleware that is specific to this router
router.use((req, res, next) => {
  console.log("Time: ", Date.now());
  next();
});

// define the home page route
router.get("/", (req, res) => {
  res.send("Birds home page");
});

// define the about route
router.get("/about", (req, res) => {
  res.send("About birds");
});

module.exports = router;
```

Then, load the router module in the app:

```javascript
const birds = require("./birds");

// ...

app.use("/birds", birds);
```

The app will now be able to handle requests to `/birds` and `/birds/about`, as well as call the timeLog middleware function that is specific to the route.


> NOTE: you can also create subroutes and mount them on another subroutes
