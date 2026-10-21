---
duration: 1h
---

# Styling in React | with Tailwind CSS

## The problem: CSS doesn't know about components

- A tree of components, rendered with the right strategy (module 6) - and the looks of 1995
- CSS predates components: one global namespace for the whole page
  - Name collisions, specificity wars, `!important` escalation
  - Styles live far from the markup they affect
  - Append-only stylesheets: nobody dares deleting a rule, who knows what it breaks
- The question of this class: where does a *component's* style live?

## The solutions ladder

### Inline styles

```jsx
<button style={{ backgroundColor: 'red', color: 'white' }}>
  Don't hit me!
</button>
```

- Colocated, scoped, works everywhere
- But: no `:hover`, no media queries, repeated on every instance

### CSS Modules

```jsx
import styles from './Button.module.css'

<button className={styles.button}>Don't hit me!</button>
```

- Class names scoped at build time (`css-1v6u1d0`), collisions solved
- But the style is still in another file, and still needs a name

### CSS-in-JS

```jsx
// Emotion, styled-components
<button css={{ backgroundColor: 'red', '&:hover span': { color: 'crimson' } }}>
  <span>Don't hit me!</span>
</button>
```

- Full colocation, dynamic styles from props - peak popularity circa 2020
- Declining since: the styles are computed at runtime in the browser, a cost, and a conflict with Server Components (module 6)
- Zero-runtime successors ([vanilla-extract](https://vanilla-extract.style/), Meta's [StyleX](https://stylexjs.com/)) compile the styles away

### Utility classes

```jsx
<button className="rounded bg-red-600 px-3 py-1 text-white hover:bg-red-700">
  Don't hit me!
</button>
```

- Single-purpose classes composed in the markup: the style lives *on* the element
- Nothing to name, nothing to keep in sync, nothing to fear deleting

## Tailwind CSS: utility-first

- The [most popular](https://2024.stateofcss.com/en-US/tools/) utility framework, the choice of this course
- The real product is the **design system**: a spacing scale, a color palette, breakpoints - consistency by constraint, not by discipline
- Variants compose: `hover:`, `focus:`, `md:` (responsive), `dark:`
- Delete a component, its styles die with it: no dead CSS
- Repeated patterns? Extract a React component first, a CSS class with `@apply` second
- v4 (2025): CSS-first configuration - `@import "tailwindcss"`, `@theme` and `@plugin` directives, no more `tailwind.config.js`
- The price: long class strings, "ugly HTML" - the bet is that developers read components, not HTML

`className` is a string like any other prop: derive it from props and state, with a helper like [clsx](https://www.npmjs.com/package/clsx):

```jsx
import clsx from 'clsx'

export default function Button({ important, children }) {
  return (
    <button
      className={clsx(
        'rounded px-3 py-1 md:px-5',
        'bg-blue-700 text-slate-100 hover:bg-blue-800',
        'dark:bg-blue-300 dark:text-slate-900',
        important && 'bg-red-600 font-bold'
      )}
    >
      {children}
    </button>
  )
}
```

In one line: **state drives the classes, classes drive the look**.

## The platform caught up

Same story as module 4 (jQuery → the DOM APIs): libraries prototype, the platform absorbs.

- Native CSS now has variables, nesting, `:has()`, container queries, cascade layers, view transitions
- Preprocessors (Sass, Less) have little left to offer, their usage is fading
- So why Tailwind in 2026? Not features: **workflow** - colocation, constraints, ubiquity
- And the AI effect again: assistants output Tailwind classes by default - most represented styling approach in training data, same self-reinforcing loop as React

## The component ecosystem

- [shadcn/ui](https://ui.shadcn.com/): components copied into the codebase and owned, built on Tailwind - the de-facto standard of AI-generated UIs
- [Tailwind Plus](https://tailwindcss.com/plus) (formerly Tailwind UI), commercial: sample code only, one of the most respected licensing models around open source
- [daisyUI](https://daisyui.com/), [Flowbite](https://flowbite.com/), [HyperUI](https://www.hyperui.dev/): free component classes and examples to learn from

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
