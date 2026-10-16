---
duration: 2h
---

# Lab: rendering strategies with Next.js

Not every page of a website has the same needs: some content is frozen, some must always be fresh, some is personalized per user. In this lab, you audit your course project, pick the proper rendering strategy for each section, and implement it with the Next.js App Router.

## Objectives

- Audit a website and associate a rendering strategy to each section
- Implement SSG, SSR, ISR and CSR with the Next.js App Router
- Measure the impact with Core Web Vitals

## Tasks

1. Audit your pages and plan a rendering strategy
2. Implement the chosen strategies
3. Bonus - measure with Lighthouse
4. Bonus - streaming with Suspense

## Prerequisites

Tag your latest commit from the previous lab with a name like `lab4` to help navigate your project. Don't forget to push the tag.

Read the [module README](./README.md) to be familiar with the strategies (CSR, SSR, SSG, ISR), the hybrid techniques (hydration, streaming, server components) and the performance metrics (TTFB, FCP, LCP, INP, CLS).

## Part 1. Audit your pages and plan a rendering strategy

For each page (or section) of your website, answer the questions from the "How to choose" section of the README:

- How often does the data change?
- Is the content the same for every user, or personalized?
- Does the page need SEO?
- How interactive is it?

Then pick a strategy and justify it. Write the result in a `docs/RENDERING.md` file, as a table. For example:

| Page                   | Data source        | Update frequency | Personalized | SEO | Strategy |
|------------------------|--------------------|------------------|--------------|-----|----------|
| `/`                    | none / static      | rarely           | no           | yes | ?        |
| `/about`               | none / static      | rarely           | no           | yes | ?        |
| `/contacts`            | none / static      | rarely           | no           | yes | ?        |
| `/articles`            | API                | often            | no           | yes | ?        |
| `/articles/:articleId` | API                | sometimes        | no           | yes | ?        |
| comments section       | API                | often            | no           | no  | ?        |

There is no single correct answer: two strategies can be defensible for the same page, what matters is your justification. Adapt the table to the actual pages of your project.

## Part 2. Implement the chosen strategies

Implement each decision of your `RENDERING.md` table. Your project must use at least once:

1. **SSG** - fully static pages (eg `/about`), rendered at build time. This is the Next.js default for pages without dynamic data: verify it with `npm run build`, the output marks each route as static (`○`/`●`) or dynamic (`ƒ`).
2. **SSG with dynamic routes** - pre-render each article page at build time with `generateStaticParams`.
3. **SSR** - a page rendered on every request (eg the article list, or any page using cookies, headers or uncached `fetch`).
4. **ISR** - a static page regenerated in the background, with a time-based `revalidate` (eg article pages refreshed every 60 seconds).
5. **CSR** - a client component (`'use client'`) fetching data in the browser (eg the comments section, a like button, a search field).

Tips:

- In the App Router, components are **server components by default**: no JS is shipped for them. Add `'use client'` only where you need interactivity (event handlers, hooks, browser APIs).
- Run `npm run build` after each change and observe how the route type evolves in the build output.
- Commit after each step following [Conventional Commits](https://www.conventionalcommits.org).

## Part 3. Bonus - measure with Lighthouse

1. Run a production build (`npm run build && npm run start`), not the dev server.
2. Audit 2 pages with different strategies using Lighthouse (Chrome DevTools) and compare TTFB, FCP, LCP, TBT and CLS.
3. Temporarily convert a static page to client-side fetching (or the reverse) and measure again. Report the comparison in `RENDERING.md`.

## Part 4. Bonus - streaming with Suspense

Add a `loading.js` file (or a `<Suspense>` boundary) to a slow page so the layout displays immediately while the data streams in. Simulate a slow data source with `await new Promise(resolve => setTimeout(resolve, 2000))`.

## References

Next.js App Router documentation for each technique of the [README](./README.md):

- **CSR / client components**: [`'use client'` directive](https://nextjs.org/docs/app/api-reference/directives/use-client), [fetching data on the client](https://nextjs.org/docs/app/getting-started/fetching-data#client-components)
- **SSR / dynamic rendering**: [server components and dynamic rendering](https://nextjs.org/docs/app/getting-started/partial-prerendering#dynamic-rendering), [fetching data on the server](https://nextjs.org/docs/app/getting-started/fetching-data#server-components)
- **SSG**: [static exports](https://nextjs.org/docs/app/guides/static-exports), [`generateStaticParams`](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)
- **ISR**: [Incremental Static Regeneration guide](https://nextjs.org/docs/app/guides/incremental-static-regeneration), [`revalidate` segment config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate), [`revalidatePath`](https://nextjs.org/docs/app/api-reference/functions/revalidatePath)
- **Hydration / server vs client components**: [server and client components](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- **Streaming SSR**: [`loading.js` and streaming](https://nextjs.org/docs/app/api-reference/file-conventions/loading), [partial prerendering](https://nextjs.org/docs/app/getting-started/partial-prerendering)
- **Server Components**: [React Server Components in Next.js](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- **Edge rendering**: [Edge runtime](https://nextjs.org/docs/app/api-reference/edge), [`runtime` segment config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#runtime)
- **Caching / DPR**: [caching in Next.js](https://nextjs.org/docs/app/deep-dive/caching)
- **Metrics**: [Core Web Vitals](https://web.dev/articles/vitals), [Lighthouse](https://developer.chrome.com/docs/lighthouse)

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
