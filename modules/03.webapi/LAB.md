---
duration: 2h
objectives:
  - Refactor a Node.js server to Express
  - Specify a REST API and drive its implementation with tests (TDD)
  - Delegate implementation to an AI agent while owning the spec, the tests and the commits
  - Collaborate on one repository with branches and pull requests
---

# Lab: a REST API, test-driven with AI

Build a REST API the way modern teams do: the specification and the tests are yours, the implementation grunt work goes to an AI agent, and every feature lands on the shared repository as a reviewed commit.

## Prerequisites

Tag your latest commit from the previous lab with a name like `lab2` to help navigate your project, and push the tag. Make it a habit: this is the last reminder, tag every completed lab from now on.

Continue in the existing code base: this lab rewrites the server of the previous one with Express.

As always: commit at every step, and keep your `README.md` up to date with the project's usage and status.

## 1. Refactor your application to Express

A warmup, by hand - no AI on this part:

1. [Install Express](https://expressjs.com/en/starter/installing.html)
2. Read the [basic routing documentation](https://expressjs.com/en/guide/routing.html)
3. Refactor your conditional routing to Express, bringing a modular source code structure with the [express.Router](https://expressjs.com/en/guide/routing.html#express-router) middleware
4. Test manually with `curl` or Postman, commit

## 2. Specify your API - by hand

Decide with your team what the API serves. The blog is the worked example of this course - **pick your own domain**: recipes, a game library, a workout tracker, a plant collection...

Write the specification in `docs/API.md`, manually - it is your contract, no AI here either:

1. The resources and their fields. For a blog: `articles` (`id`, `title`, `content`, `author`, `date`) and `comments` (`id`, `articleId`, `content`, `author`, `timestamp`).
2. The routes as a table: method, path, request body, response, status codes. At minimum: full CRUD on the main resource, plus one nested resource. For a blog:

   | Method | Path                     | Body             | Response      | Status    |
   |--------|--------------------------|------------------|---------------|-----------|
   | GET    | `/articles`              | -                | all articles  | 200       |
   | POST   | `/articles`              | article (no id)  | created article | 201     |
   | GET    | `/articles/:id`          | -                | one article   | 200 / 404 |
   | PUT    | `/articles/:id`          | article          | updated article | 200 / 404 |
   | DELETE | `/articles/:id`          | -                | -             | 204 / 404 |
   | GET    | `/articles/:id/comments` | -                | its comments  | 200 / 404 |
   | POST   | `/articles/:id/comments` | comment (no id)  | created comment | 201 / 404 |

3. Storage is a hard-coded in-memory object - the database comes in module 8. Generate ids with the native [`crypto.randomUUID()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID).

## 3. The TDD loop - with an agent

Set up the test harness: [`node:test`](https://nodejs.org/api/test.html) with [Supertest](https://www.npmjs.com/package/supertest) as a dev dependency, and a `test` script in `package.json`. Supertest needs to import the Express `app` without starting it: export the app from one module, call `listen` in another.

Here is one full cycle, on the blog example:

1. **Red** - write the test by hand, in `test/articles.test.js`:

   ```js
   import app from '../app.js'
   import request from 'supertest'
   import assert from 'node:assert'
   import test from 'node:test'

   test('articles: create then list', async () => {
     // Create an article
     await request(app)
       .post('/articles')
       .send({ title: 'My article', content: '...', author: 'Alice' })
       .expect(201)

     // It shows up in the list
     const { body: articles } = await request(app)
       .get('/articles')
       .expect('Content-Type', /json/)
       .expect(200)

     assert.strictEqual(articles[0].title, 'My article')
   })
   ```

   `npm test` fails: red.

2. **Green** - prompt the agent, for example: *"Make `test/articles.test.js` pass by implementing `POST /articles` and `GET /articles` in the Express app, following `docs/API.md`. Minimal code, don't touch the tests."* Run `npm test`: green.

3. **Refactor** - review the generated code, ask for cleanups if needed, tests stay green.

4. **Commit**: `feat(api): create and list articles`.

Now split the endpoints of your spec among the team members. Each member covers their routes by repeating the loop, one route at a time, on their own branch:

1. **Red** - write the test yourself, covering the behavior *and* its error case (eg the 404). Run the suite: it must fail.
2. **Green** - hand over to an AI agent (Copilot, Claude, Cursor, ...): give it the spec and the failing test, ask for the minimal implementation. Run the suite: it must pass.
3. **Refactor** - read the generated code as a reviewer. Duplicated logic, dead code, odd naming? Ask the agent to refactor; the tests must stay green.
4. **Commit** - one feature, one commit: `feat(api): list articles`, `feat(api): delete an article`, ...
5. **Merge** - open a pull request, reviewed by at least one teammate. Keep `main` green: run `npm test` before approving.

Rules of the game:

- The agent never writes or edits tests. If a test must change, the spec was wrong: fix `docs/API.md` first, in its own commit.
- Never weaken a test to make an implementation pass.

At some point, two of you will touch the same file - good. Resolve the merge conflict together (lab 1 skills), rerun the tests, and move on.

## 4. Bonus - tests on every pull request

Automate the merge gate with a [GitHub Actions workflow](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-nodejs) running `npm test` on each pull request, and make it [required to merge](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches). Nobody - human or agent - merges red.

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
