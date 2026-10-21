---
duration: 2h
objectives:
  - Install Tailwind CSS in a Next.js application
  - Style a responsive layout with utility classes
  - Build stateful, styled components respecting the server/client boundary
---

# Lab: style your project with Tailwind CSS

Your project has pages, routes and rendering strategies - and no face. In this lab, you install Tailwind CSS, style your layout, and give the project its first fully featured components: stateful, interactive, styled, and placed on the right side of the server/client boundary you learned in the rendering lab.

## 1. Install Tailwind CSS

Follow the [official installation guide](https://tailwindcss.com/docs/installation/framework-guides/nextjs) to install Tailwind CSS inside Next.js. Since you already have a Next.js application, start at step 2.

If you wrote any CSS so far, remove it: `app/globals.css` must contain the `@import "tailwindcss";` directive, and little to nothing else.

Verify the installation: add `className="underline"` to any text of your home page, it must display underlined.

## 2. Style the layout

Style your `Header`, `Footer` and `Nav` components, then give every page a consistent look with utility classes. The pages can keep their placeholder content: it is the frame that matters.

Requirements:

- Stick to the design system: pick 2-3 colors from the [palette](https://tailwindcss.com/docs/colors) and a consistent spacing rhythm
- The layout must adapt to mobile with the [responsive variants](https://tailwindcss.com/docs/responsive-design) (`sm:`, `md:`, ...)

Use the [Tailwind CSS documentation](https://tailwindcss.com) (the `ctrl-k` search is your friend), and get inspiration from free galleries like [HyperUI](https://www.hyperui.dev/).

## 3. Custom classes with the `@apply` directive

Your pages all have a title, style them once. Inside `app/globals.css`, create a `wt-title` class ("wt" stands for Web Tech):

```css
@layer components {
  .wt-title {
    @apply text-3xl font-bold underline mb-10;
  }
}
```

Add the `wt-title` class to every `h1` of your pages. Remember the class rule of thumb: extract a React component first, an `@apply` class second - a shared `<PageTitle>` component would work too.

## 4. Fully featured components

Time to make the project interactive. You will build components that **manage their own state** and are **styled in all their states** (hover, focus, disabled, open/closed, invalid, ...).

For each component, apply the rendering lab: decide whether it is a server or a client component, add `'use client'` only where state or event handlers require it, and keep it as deep in the tree as possible - the page around it must stay a server component.

Build these two:

1. **Mobile navigation**: below the `md:` breakpoint, the nav menu collapses behind a hamburger button, opened and closed with `useState`. The `Nav` becomes a client component, the layout around it stays server.
2. **Contact form** on your contacts page: controlled inputs (module 4), client-side validation with visible feedback (invalid fields styled, submit button disabled while invalid), and a success state on submission. Log the data to the console for now: module 8 plugs a database behind it. The [@tailwindcss/forms](https://github.com/tailwindlabs/tailwindcss-forms) plugin (registered with [`@plugin`](https://tailwindcss.com/docs/functions-and-directives#plugin-directive)) gives you sane input defaults.

And at least one of your choice, for example:

- A **search field** on your collection page, filtering the displayed items as you type
- A **like button** on your item pages, with an optimistic counter
- A **theme toggle** switching the `dark:` variant on and off

Finally, update `docs/ARCHITECTURE.md`: mark each component server or client. Run `npm run build` and check against your `docs/RENDERING.md`: adding interactivity must not have turned your static pages dynamic.

## 5. Bonus - dark mode

Extend the theme toggle to the whole site: support the `dark:` variant on the layout and every component, persist the choice in `localStorage`, and make sure the page doesn't flash on reload. The [dark mode documentation](https://tailwindcss.com/docs/dark-mode) covers the custom variant setup.

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
