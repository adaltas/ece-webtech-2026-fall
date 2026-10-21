# Demo: a click counter, three ways

The same application - a button that increments a counter each time you click it - implemented three times. Open each `index.html` directly in your browser: no build step, no `npm install`.

1. [Demo 1. Plain JavaScript](demo-1/index.html) - the state lives in a variable, **we** are responsible for keeping the DOM in sync with it
2. [Demo 2. React with `React.createElement()`](demo-2/index.html) - the state is declared, **React** keeps the DOM in sync, but building the tree by hand is verbose
3. [Demo 3. React with JSX](demo-3/index.html) - same component, written in JSX and transpiled to `createElement()` calls by Babel, live in the browser

In demo 3, open the browser developer tools and inspect the generated code to see the `createElement()` calls that Babel produced from the JSX.

> [!NOTE]
> Transpiling in the browser with Babel is for demonstration only. Real projects transpile ahead of time with a build tool like [Vite](https://vite.dev/).
