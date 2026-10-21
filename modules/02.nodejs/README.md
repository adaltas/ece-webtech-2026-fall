---
duration: 1h
---

# Getting started with JavaScript, Node.js, and Asynchronous Programming

JavaScript was born in the browser, and in 2009 it escaped to the server. This class follows the escape: the language and its history, the runtime that freed it, the modules and packages that organize its code, and the single-threaded asynchronous model that made it a serious server platform.

## The server-side landscape

Every era had its server language. JavaScript arrived last, with a twist: it is also the language of the browser.

Language / Framework | Start | Used for
--- | --- | ---
Perl / CGI | 1990s | Initial CGI scripts
PHP | 1995 | Dynamic sites, WordPress, CMS
Java / Servlets, Spring | 1997 / 2004 | Enterprise apps, REST APIs
Ruby on Rails | 2004 | Rapid MVPs, startups
Python / Django | 2005 | Full‑stack apps, auto‑generated admin
Python / Flask | 2010 | Microservices, lightweight APIs
Python / FastAPI | 2019 | High-performance APIs, async
JavaScript / Node.js + Express | 2009 | Event-driven apps, real‑time, full‑stack JS

### Why JavaScript for this course?

- The language of the browser, and among the [most used languages overall](https://survey.stackoverflow.co/2025/technology)
- Full-stack JavaScript: Same language for front-end and back-end
- Rich ecosystem: npm, frameworks (Express, Next.js, etc.) and community support
- TypeScript: Optional static typing for better tooling and maintainability

Highlights:

- No relation to Java
- Multi-paradigm: scripting, object-oriented, functional, imperative, event-driven
- Single-threaded and synchronous (asynchronous I/O with event loop)
- Runs natively in web browsers

## A brief history of JavaScript

- 1995: Created by Brendan Eich at Netscape (in 10 days)
- Named Mocha, then LiveScript, then JavaScript
- 1996: Shipped with IE3 in 1996 as JScript (Sun - now Oracle - owned the JavaScript trademark)
- 1996: Standardized as ECMAScript (ES) by ECMA International
- 1997: ES1 released
- 2009: Node.js created by Ryan Dahl - **the escape**: JavaScript runs on the server
- 2010: npm (Node Package Manager) introduced
- 2015: ES6 released, major update with classes, modules, arrow functions, promises
- 2020: Node.js 14.0.0 released, with native ES modules
- Since ES6, a new version is released every year
- 2020s: alternative runtimes - [Deno](https://deno.com/) (2020, by Node's creator) and [Bun](https://bun.sh/) (2023); Node.js answers with a built-in test runner, watch mode, and native TypeScript type-stripping

## Engine, runtime, Node.js

What did the escape take? Separating the language from the browser:

- Engine: Executes JavaScript code, implementation of ECMAScript (ex: V8, SpiderMonkey, JavaScriptCore)
- Runtime: Engine + API and libraries for additional functionality (ex: Chrome, Firefox, Node.js)
- Node.js: JavaScript runtime built on Chrome's V8 engine, allows running JavaScript on the server side
  - Same language, new powers: APIs for the file system, networking, processes - everything the browser forbids

## JavaScript modules

A server application is more than one file. Modules are reusable pieces of code that can be imported and exported - and Node.js introduced its own system before the standard existed:

- CommonJS: Module system historically used in Node.js (uses `require` and `module.exports`)
- ES Modules: Standardized module system (uses `import` and `export`)

### CommonJS vs ES Modules

- CommonJS:
  - Synchronous loading
  - Uses `require()` to import modules
  - Uses `module.exports` or `exports` to export modules
  - Example:

    ```js
    // math.js
    function add(a, b) {
      return a + b;
    }
    module.exports = { add };

    // main.js
    const { add } = require('./math');
    console.log(add(2, 3)); // 5
    ```

- ES Modules:
  - Asynchronous loading
  - Uses `import` to import modules
  - Uses `export` to export modules
  - Example:

    ```js
    // math.js
    export function add(a, b) {
      return a + b;
    }

    // main.js
    import { add } from './math.js';
    console.log(add(2, 3)); // 5
    ```

- ES Modules are the standard: use them for new code. In Node.js, opt in with `"type": "module"` in `package.json` (or the `.mjs` extension)

## JavaScript packages

More than one file, and more than one author: code is shared as packages.

- What other languages call a library
- A directory that contains one or more `.js` files
- A directory that contains a `package.json` file at its root, describing:
  - General information: `name`, `description`, `version`, `license`
  - Is the project private: `private`
  - Scripts and commands: `scripts`
  - Dependencies: `dependencies` and `devDependencies`
  - Full properties list: <https://docs.npmjs.com/files/package.json>
- Use the `npm init` command to create a new package

Package structure example:

```txt
ece-webtech/
├── index.js
└── package.json
```

### `package.json` file

```json
{
  "name": "ece-webtech",
  "description": "Node.js project for ECE class",
  "version": "0.0.0",
  "license": "UNLICENSED",
  "private": false,
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

## Dependencies

- Packages can depend on other packages (dependencies)
- Declared in the `package.json` file under `dependencies` and `devDependencies`
- Installed using a package manager (e.g., npm, yarn, pnpm)
- Installed dependencies are stored in the `node_modules` directory
- Each dependency can also possess its own set of dependencies, leading to a hierarchy.

### Declare dependencies

In the `package.json` file:

- `dependencies`: Essential for the app to operate.
- `devDependencies`: Required only during development like test runners or documentation tools.

Example of declaration in package.json:

```js
{ "dependencies": {
  "foo": "^1.0.0", // dependency with a version constraint
  "baz": "file:/path/to/file", // local dependency
  "bar": "http://web.site/my.tar.gz" // remote dependency
  },
  "devDependencies": {
    "test": "1.0.0"
  }
}
```

Refer to [package.json documentation](https://docs.npmjs.com/files/package.json) for more details.

### Package managers

- Tools that automate the process of installing, upgrading, configuring, and removing packages.
- Help manage project dependencies efficiently.
- Ensure consistent environments across different setups.
- Main package managers for JavaScript:
  - NPM (Node Package Manager): Shipped with Node.js, most widely used.
  - Yarn: Developed by Facebook to address some NPM issues. Since then, both are quite similar.
  - pnpm: Manages dependencies differently, more efficient in some cases.

#### Example installation with NPM

Initialize a new Node.js project:

```sh
npm init -y
```

Install two packages:

```sh
npm install package1 package2
```

The project structure will look like this:

```txt
ece-webtech/
├── index.js
├── node_modules/
│   ├── package1/
│   │   ├── index.js
│   │   └── package.json
│   └── package2/
│       ├── index.js
│       └── package.json
├── package.json
└── package-lock.json (or yarn.lock, or pnpm-lock.yaml)
```

In the `package.json` file:

```json
{
  "name": "ece-webtech",
  "description": "Node.js project for ECE class",
  "version": "0.0.0",
  "license": "UNLICENSED",
  "private": false,
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "package1": "^1.0.0",
    "package2": "^2.0.0"
  },
  "devDependencies": {}
}
```

### Versioning with Semver

Semantic Versioning (Semver) is a convention to version packages.

- Version format is `X.Y.Z`
  - X: Major - introduces breaking changes.
  - Y: Minor - adds new features while maintaining backward compatibility.
  - Z: Patch - primarily for bug fixes.
- Version 0.Y.Z indicates initial development and might be unstable.

Use the following syntax to specify a version in the `package.json` file:

- `1.2.3` - exact version
- `~1.2.3` - major and minor versions must match
- `^1.2.3` - major version must match
- `>1.2.3` - any superior to 1.2.3
- `1.2.x` - any 1.2 version

Learn more about [Semantic Versioning](https://semver.org/).

Using conventional commit messages and tools like [semantic-release](https://semantic-release.gitbook.io/semantic-release/) can help automate versioning.

### Deterministic installs with lock files

- Lock files, such as package-lock.json for NPM and yarn.lock for YARN, ensure consistent dependency versions across installations.
- Helps avoid discrepancies between developer environments, preventing the "but it works on my computer!" issue.

## Asynchronous programming

The price of the escape: one single thread serves every request. Blocking it freezes the whole server, so nothing may wait.

- Synchronous: Blocking, sequential execution (e.g., traditional function calls)
- Asynchronous: Non-blocking, allows other code to run while waiting for operations (e.g., I/O operations)
- Powered by the [event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model): waiting operations are queued, their continuations executed when ready

Three patterns, from historical to current:

- **Callbacks**: a function passed as argument, invoked when the operation completes. Nesting them leads to the infamous "callback hell"
- **Promises** (ES6): an object representing an eventual result, chained with `.then()` and `.catch()`
- **`async`/`await`** (ES2017): syntactic sugar over promises, asynchronous code that reads synchronously - the modern default

References: [Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS) (MDN), [Promises, async/await](https://javascript.info/async) (javascript.info).

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
