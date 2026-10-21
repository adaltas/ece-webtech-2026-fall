---
duration: 2h
objectives:
  - Build and nest React components with JSX
  - Pass data through props and manage state with useState
  - Explain why immutability matters in React
---

# Lab: React fundamentals with Tic-Tac-Toe

React is best learned by building something small. In this lab, you follow the [official React Tic-Tac-Toe tutorial](https://react.dev/learn/tutorial-tic-tac-toe) to practice the core concepts of the class: components, JSX, props, state, and immutability.

> [!IMPORTANT]
> Complete parts 1 to 3 **without AI code generation**. In your career, AI will write a lot of React for you: this lab is where you build the intuition needed to judge what it writes. AI enters the game in part 4, as a reviewer of your learning journey.

## 1. Set up a local React project

The official tutorial can be followed in the browser with CodeSandbox. However, working locally allows to keep your code in your repository.

1. Under your repository root, create a React application with [Vite](https://vite.dev/guide/):

   ```bash
   npm create vite@latest tic-tac-toe -- --template react
   cd tic-tac-toe
   npm install
   npm run dev
   ```

2. Open the ["Setup for the tutorial"](https://react.dev/learn/tutorial-tic-tac-toe#setup-for-the-tutorial) section, look at the starter code sandbox, and reproduce it locally: replace the content of `src/App.jsx` with the `App.js` starter code and the content of `src/index.css` with the `styles.css` starter code.

3. Commit your changes.

## 2. Complete the official Tic-Tac-Toe tutorial

Follow every section of the [tutorial](https://react.dev/learn/tutorial-tic-tac-toe).

Don't copy-paste the final result: type the code and make sure you understand each step. Take the time to read the "Why immutability is important" section carefully, it is a recurring interview question and a common source of React bugs.

Commit at the end of each section.

## 3. Bonus - improve the game

The tutorial ends with a [list of improvements](https://react.dev/learn/tutorial-tic-tac-toe#wrapping-up) ordered by increasing difficulty. Implement as many as you can.

## 4. Bonus - AI code review

Now, and only now, bring in an AI assistant (Claude, ChatGPT, Copilot, ...):

1. Ask it to **review your code** and suggest improvements. For each suggestion, decide whether you agree and why - you are the reviewer of the reviewer. Apply the ones you keep in a dedicated commit.
2. Sabotage your game: replace one `squares.slice()` copy with a direct mutation of the `squares` array, and observe how time travel breaks. Ask the AI to find the bug from the symptom alone ("time travel shows the wrong board"). Did it find it faster than you would have?
3. Ask the AI to implement one of the part 3 improvements you didn't do. Read its code critically: would you have accepted it in part 2?

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
