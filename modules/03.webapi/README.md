---
duration: 1h
---

# Web API | with REST and Express.js

The module will introduce the Express framework to write a web application that exposes REST API services.

## What’s an API?

- Application Programming Interface
- Is a set of definitions and protocols for building and integrating software
- Is a "contract" between an information provider and an information user
- API in web (mostly) - REST API

## How to use an API?

- Combination of 2 *microservices*:
  - **Back-end** exposes REST API
  - **Front-end** (web pages, mobile apps) consumes REST API

## REST API

- REST - REpresentational State Transfer
- Exposes a set of HTTP routes
- Uses [HTTP Verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) (GET, POST, PUT, DELETE, ...)
- Clients send requests to communicate
- Usually communicating in JSON

REST API example: <https://petstore.swagger.io/>

## REST API Methods

Uses standard HTTP methods:

- **GET** - retrieve a specific resource (by id) or a collection of resources
- **POST** - create a new resource
- **PUT** - update a specific resource (by id)
- **PATCH** - partially update a specific resource (by id)
- **DELETE** - remove a specific resource by id

## GraphQL

- Self-documenting APIs to end users
- Enhanced flexibility with selective properties and relations
- Fewer round trips: one query fetches many related resources
- Speed of integration for end-users, in particular when working with different back-end teams
- DBnomics demo: [Expose an OpenAPI schema with GraphQL](https://dbnomics.adaltas.com/)

## What is Express?

- Minimalist framework for Node.js apps
- Provides features for web app development
- Create robust APIs
- The de-facto standard since 2010, version 5 released in 2024

[Express guide](https://expressjs.com/en/guide/routing.html)

## Create a basic server

- Manually: use Node.js `http` module
- With Express:

```js
import express from 'express'
const app = express()

app.set('port', 8080)

app.listen(
  app.get('port'), 
  () => console.log(`server listening on ${app.get('port')}`)
)
```

## Routing

- Manually: parse the URL and apply the corresponding logic
- With Express:

```js
app.get('/', function (req, res) {
  // GET
})

app.post('/', (req, res) => {
  // POST
})

app
  .put('/', function (req, res) {
    // PUT
  })
  .delete('/', (req, res) => {
    // DELETE
  })
```

## Routing parameters

URIs can contain parameters that are indicated by `:` character.

Example: in `http://localhost:3000/hello/John`, `John` is a parameter.

```js
app.get(
  '/hello/:name', 
  function (req, res) {
    res.send("Hello " + req.params.name)
  }
)
```

[Read more](https://expressjs.com/en/guide/routing.html)

## Query parameters

- are optional
- passed in the URL after the `?` character
- separated by `&` character
- be specified multiple times

```js
// GET /hello?name=John
app.get('/hello', function (req, res) {
  res.send("Hello " + req.query.name)
})
```

## Testing an API

### By hand

1. The browser: the easiest way to test a `GET` - type the URL, read the response (Firefox even pretty-prints JSON). Limited to `GET`, no headers, no body

2. `curl` Bash command. Examples:

    ```bash
    # Sending GET request
    curl -X GET http://localhost:3000/user
  
    # Sending POST request
    curl -X POST http://localhost:3000/user \
    -H 'Content-Type: application/json' \
    -d '{"username":"myuser","email":"myuser@example.com"}'
    ```

3. GUI clients, to craft and replay requests (method, body, headers):

    - Open source: [Yaak](https://yaak.app/) (desktop, by the creator of Insomnia), [Hoppscotch](https://hoppscotch.io/) (in the browser)
    - [Postman](https://www.postman.com/): the long-time industry standard - does everything, at the price of a heavier app and a strong pull toward its cloud accounts

Manual testing doesn't scale: it is repeated at every change. Automate it.

### Automated: the kinds

- **Unit**: one function or module in isolation - fast, no network, no disk
- **Integration**: several pieces together, eg an HTTP request through the real routes to a response - the level of this module
- **End-to-end (E2E)**: the whole application driven like a user would, through a real browser ([Playwright](https://playwright.dev/), [Cypress](https://www.cypress.io/))
- The higher the level, the more realistic and the slower: a healthy suite is a pyramid - many unit, some integration, few E2E

### Writing tests

- A test is just a function scheduled by a test runner; tests can be grouped, filtered, and skipped
- Node.js has a built-in test runner since v18: [`node:test`](https://nodejs.org/api/test.html), no dependency needed; ecosystem alternatives: [Vitest](https://vitest.dev/), [Jest](https://jestjs.io/), [Mocha](https://mochajs.org/) (historic)
- A test must throw to fail: assertions throw when a check doesn't hold
  - Node.js has a built-in [`node:assert`](https://nodejs.org/api/assert.html) module
  - [Supertest](https://www.npmjs.com/package/supertest) sends real requests to an Express app and asserts on the responses
- Structure each test as: setup, execution, validation, cleanup
- Avoid the anti-patterns: tests depending on each other or on leftover state, over-inspecting, slow tests

### Test-driven development (TDD)

Write the test before the code, then loop:

1. **Red** - write a failing test specifying one behavior
2. **Green** - write the minimal implementation that makes it pass
3. **Refactor** - clean up, the tests must stay green

- The test suite is the executable specification of the API, and a regression safety net
- One cycle = one feature = one commit: a readable history
- In the age of AI, TDD is a division of labor: the tests are the contract the developer owns and writes, the implementation can be delegated to an agent - the suite tells both when the code is right

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
