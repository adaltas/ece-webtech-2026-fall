---
duration: 2h
objectives:
  - Initialize an npm project and publish it to GitHub
  - Write and run Node.js scripts
  - Build a Node.js HTTP server with multiple routes
---

# Lab: getting started with Node.js & Git

This lab starts a new Node.js project which serves as the basis for the following courses: every week, incremental features are completed toward a working application. In the end, it becomes your final project - the final grade reflects the delivered project, as well as its Git history.

## Prerequisites

- The lab 1 environment: UNIX-like system, Git, team repository.
- Node.js LTS installed following the [setup guide](../01.introduction/SETUP.md). Verify with `node --version` and `npm --version`.

## 1. Create a simple Node.js script

### Start working with a text editor

Now, we start using a text editor or IDE (Atom, VS Code, WebStorm, or up to your choice).

Open a project folder in your editor. You also can use bash commands for opening it. Being under the root of the project directory, run one of the commands:

```bash
# For VS Code
code .
# For power users
vim .
```

### Create a script

Create a file `index.js` with the following content:

```js
console.log("Hello Node.js!")
```

Run the Node.js script in the terminal:

```bash
node index.js
```

It will print the message `Hello Node.js!`.

### Initialize a Node.js project

Run the command:

```bash
npm init
```

It will create a `package.json` file for you. Later, you can manually modify the content respecting the [JSON format](https://en.wikipedia.org/wiki/JSON). For example, these values:

- `author`
- `description`

Add the `"type": "module"` property: as seen in class, ES Modules are the standard, this enables `import`/`export` in the project.

### Define an NPM script

Add the script `start` to the `package.json` file like this:

```json
{
  ...
  "scripts": {
    "start": "node index.js"
  },
  ...
}
```

Run the NPM script with the command:

```bash
npm run start
# or
npm start
```

It will do the same as in step 2.

### Initialize a Git repository

Run the command:

```bash
git init
```

It will create a `.git` folder for you.

Create a `.gitignore` file to avoid committing unnecessary files like:

```txt
.DS_Store
/node_modules
```

Perform the first commit:

```bash
git add .
git commit -m "Initial commit"
```

## 2. Create an HTTP server

### Basic HTTP server implementation

Modify the `index.js` file with the following content

```js
// Import a module
import http from 'node:http'

// Declare an http server
http.createServer(function (req, res) {

  // Write a response header
  res.writeHead(200, {'Content-Type': 'text/plain'})

  // Write a response content
  res.end('Hello World\n')

// Start the server
}).listen(8080)
```

Read and understand each line in this code using the [`http` Node.js official module documentation](https://nodejs.org/api/http.html).

### Run HTTP server

Run the command:

```bash
npm run start
```

It will start a web server accessible on <http://localhost:8080>:

- open it in a browser
- or `curl localhost:8080` in a terminal to get the home page content

### Define callback function

Rewrite the code to define a callback function

```js
const serverHandle = function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'})
  res.end('Hello World\n')
}

http
.createServer(serverHandle)
.listen(8080)
```

Don't forget to restart the server. To terminate a blocking process in the terminal use the combination of keys `Ctrl + C`. Start it again with `npm start`.

### Sending back HTML

Change the content type & response content:

```js
// Define a string constant concatenating strings
const content = '<!DOCTYPE html>' +
'<html>' +
'    <head>' +
'        <meta charset="utf-8" />' +
'        <title>ECE AST</title>' +
'    </head>' + 
'    <body>' +
'       <p>Hello World!</p>' +
'    </body>' +
'</html>'

const serverHandle = function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'})
  res.write(content)
  res.end()
}

http
.createServer(serverHandle)
.listen(8080)
```

### Get the current path

A path is the part of the URL that comes after the domain name. For example, in `http://localhost:8080/my/path`, the path is `/my/path`.

Enrich the previous code with the following - the [`URL` class](https://developer.mozilla.org/en-US/docs/Web/API/URL) is global, no import needed:

```js
const serverHandle = function (req, res) {
  // Retrieve and print the current path
  const path = new URL(req.url, `http://${req.headers.host}`).pathname
  console.log(path)

  res.writeHead(200, {'Content-Type': 'text/html'})
  res.write(path)
  res.end()
}

http
.createServer(serverHandle)
.listen(8080)
```

Access to your local website by different URLs like `localhost:8080` and `localhost:8080/my/path` and see what is printed in the terminal.

### Get query parameters

Web URLs can be enriched with query parameters, which come after the `?` and are separated by `&`, formatted as `key=value`.

```txt
http://my.site/my/page.html?username=toto&password=lulu
```

The `URL` class parses them and exposes them as [`searchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URL/searchParams).

Enrich the previous code with the following:

```js
const serverHandle = function (req, res) {
  // Retrieve and print the query parameters
  const { searchParams } = new URL(req.url, `http://${req.headers.host}`)
  console.log(Object.fromEntries(searchParams))

  res.writeHead(200, {'Content-Type': 'text/html'})
  res.write(content)
  res.end()
}

http
.createServer(serverHandle)
.listen(8080)
```

Access your local website by different URLs like `localhost:8080/?name=John&email=john@email.com`, and see what is printed in the terminal.

### Basic routing example

Enrich the previous code with the following:

```js
const serverHandle = function (req, res) {
  const route = new URL(req.url, `http://${req.headers.host}`)
  const path = route.pathname
  const params = route.searchParams

  res.writeHead(200, {'Content-Type': 'text/plain'})

  if (path === '/hello' && params.has('name')) {
    res.write('Hello ' + params.get('name'))
  } else {
    res.write('Hello anonymous')
  }

  res.end()
}
```

Access to your local website by different URLs like `localhost:8080/hello?name=John`.

### Organize the source code in a module

Create `handles.js` and `index.js` files and reorganize the previous code like:

```js
// ./handles.js
// Necessary imports
export const serverHandle = function (req, res) {
  // ...
}
```

```js
// ./index.js
import http from 'node:http'
import { serverHandle } from './handles.js'

http
.createServer(serverHandle)
.listen(8080)
```

## 3. Integrate Nodemon

Tired of restarting your webserver after every modification of the source code? Let's fix it!

Nodemon is a simple utility that watches your development files and restarts the server automatically upon saving, eliminating the need to restart the server manually.

### Install Nodemon

Run:

```bash
npm install nodemon
# or
yarn add nodemon
# then
npx nodemon index.js
# npx avoids running `./node_modules/.bin/nodemon index.js`
```

Now, it will restart the web server when the file is updated. There is no need to restart it manually to apply modifications of code. Just refresh the page in a browser.

> [!IMPORTANT]
> Don't forget to define a `.gitignore` to prevent committing the `node_modules` folder.

### Define an NPM script in `package.json`

Enrich your `scripts` in `package.json` with:

```json
"scripts": {
  ...
  "dev": "nodemon index.js"
  ...
}
```

And then always run when developing:

```bash
npm run dev
```

## 4. Create a basic application with multiple routes

Create an application with 3 routes:

1. `/` explains how `/hello` works (containing the links)
2. `/hello` takes a `name` query parameter and:
    - random names reply `hello [name]`
    - your own name replies with a short intro of yourself
3. Any other path replies a 404 code with a not found message

## 5. Reading from a JSON file

1. Create a subfolder with the name `content` and create a JSON file `about.json` inside it with the example content like this:

    ```json
    {
      "title": "About",
      "content": "Example content here.",
      "author": "Your Name",
      "date": "27/09/2022"
    }
    ```

1. Create the route `/about` displaying the content of this JSON file:

    - read the file with [`readFileSync`](https://nodejs.org/api/fs.html#fsreadfilesyncpath-options) from `node:fs` and parse it with `JSON.parse()`
    - choose a proper `Content-Type` for displaying JSON

1. Create dynamic routing.

Parse the path and verify if a JSON file exists in the `content` folder. If yes, print its content. If no, redirect to the 404 error page.

Bonus - practice the asynchronous patterns from the class: read the file with the callback-based [`fs.readFile`](https://nodejs.org/api/fs.html#fsreadfilepath-options-callback), then rewrite it with [`node:fs/promises`](https://nodejs.org/api/fs.html#fspromisesreadfilepath-options) and `await`. Compare the three versions.

## 6. Publish your project to GitHub

### Document your project

Update the `README.md` file. It should at least contain:

- Project title
- Short introduction
- Prerequisites
- Installation instructions
- Usage instruction with simple (and advanced) examples
- List of contributors

### Push it to GitHub

Commit your changes, use your **PRIVATE** group repository on GitHub, and push it.

## 7. Bonus - cultivate yourself

- [Learn Markdown](https://www.markdownguide.org/)
- [How to write README](https://dev.to/scottydocs/how-to-write-a-kickass-readme-5af9)

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
