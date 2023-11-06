---
title: "Quick Start"
description: ""
summary: ""
date: 2023-11-06T01:59:02+05:30
lastmod: 2023-11-06T01:59:02+05:30
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "quick-start-f66a0eab267a121191296cf267329827"
weight: 999
toc: true
---

Welcome to OvenJoy, a lightweight and flexible web framework for Bun. OvenJoy is designed to simplify the process of building web applications and APIs by providing you with powerful routing and middleware capabilities. In this quick start guide, we'll walk you through the steps to get started with OvenJoy.

## Prerequisites

Before you begin, make sure you have the following prerequisites installed on your system:

- Bun: is all you need to get started.

```bash
curl https://bun.sh/install | bash
```

and init the Bun project

```bash
bun init
```

## Installation

You can install OvenJoy in your Bun project using bun package manager. Open your terminal and run the following command:

```bash
bun add ovenjoy
```

This will add OvenJoy to your project's dependencies.

## Creating a Simple OvenJoy Application

Let's create a simple OvenJoy application to demonstrate its basic features. Create a new JavaScript file, e.g., app.js, and follow along:

```javascript
// Import the OvenJoy framework
import OvenJoy from "ovenjoy";

// Create an OvenJoy application
const app = OvenJoy();

// Define a route
app.get("/", (req, res) => {
  res.json({ message: "Hello, OvenJoy!" });
});

// Start the server
app.listen(3000, () => {
  console.log("OvenJoy server is running on port 3000");
});
```

Start a development server by:

```bash
bun app.js
```

Open your browser and go to http://localhost:3000.

You should see your server is running.
