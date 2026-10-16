---
duration: 1h
---

# Rendering strategies and data fetching | with Next.js

## Performance metrics

Loading:

- **TTFB** (Time To First Byte): delay between the request and the first byte of the response, reflects network latency + server processing time (very sensitive to SSR vs SSG/CDN)
- **FCP** (First Contentful Paint): first content element displayed (text, image, SVG), "something finally shows up"
- **LCP** (Largest Contentful Paint): largest element visible in the viewport is rendered, proxy for "main content is loaded", Core Web Vital, good ≤ 2.5s
- **TBT** (Total Blocking Time): sum of main thread blocking times (tasks > 50ms) between FCP and TTI, lab metric correlated to interactivity
- **TTI** (Time To Interactive): page is durably interactive (JS loaded, main thread free), historical Lighthouse metric, deprecated in favor of TBT and INP

Interactivity and stability:

- **INP** (Interaction to Next Paint): latency of all interactions (click, tap, keyboard) during the visit, roughly the worst one, Core Web Vital (replaced FID), good ≤ 200ms
- **CLS** (Cumulative Layout Shift): sum of unexpected layout shifts (image without dimensions, inserted ad…), unitless score, Core Web Vital, good ≤ 0.1

Measurement: lab data (Lighthouse, WebPageTest) vs field data (CrUX, web-vitals, RUM). Core Web Vitals are evaluated on field data at the 75th percentile.

## Foundational strategies

### CSR - Client-Side Rendering

- Server sends a nearly empty HTML + a JS bundle
- Browser downloads, executes the JS, calls the APIs, then builds the DOM
- Pros: smooth navigation after load (SPA), light server, good TTFB
- Cons: slow first paint (FCP/LCP), fragile SEO, depends on JS
- Eg [Vite + React](https://vite.dev/guide/) (pure SPA)

### SSR - Server-Side Rendering

- HTML is generated on every request, server-side
- Pros: content immediately visible, good SEO, fresh data, personalized or secured on-demand
- Cons: higher TTFB, server load, no trivial caching, source of errors if generation fails (eg broken database query)
- Eg full-stack frameworks: [Next.js](https://nextjs.org/docs/app/getting-started/partial-prerendering#dynamic-rendering), [Nuxt](https://nuxt.com/docs/guide/concepts/rendering), [SvelteKit](https://svelte.dev/docs/kit/page-options), [Remix](https://remix.run/docs/en/main/discussion/introduction)

### SSG - Static Site Generation

- HTML is generated at build time, then served as-is via a CDN
- Pros: as fast as it can get, cheap, secure, no state, easy to distribute
- Cons: content frozen until the next build, build time explodes with the number of pages
- Eg pure SSG: [Eleventy](https://www.11ty.dev/docs/), [Hugo](https://gohugo.io/documentation/); also [Next.js static exports](https://nextjs.org/docs/app/guides/static-exports)

## Hybrid strategies

### Hydration

- HTML (SSR/SSG) is rendered server-side, then JS "wakes up" the page by attaching event handlers
- Pitfall: the page is visible but not yet interactive (uncanny valley), gap between FCP and TTI/INP
- Risk of hydration mismatch (server and client render differently)

Techniques reducing or avoiding the hydration cost:

- **Selective / Progressive hydration**: hydrate per component, by priority or visibility. Eg [Astro `client:*` directives](https://docs.astro.build/en/reference/directives-reference/#client-directives) (`client:idle`, `client:visible`…)
- **Islands architecture**: static page with interactive "islands" hydrated individually, very little JS shipped. Eg [Astro](https://docs.astro.build/en/concepts/islands/), [Fresh](https://fresh.deno.dev/docs/concepts/islands)
- **Resumability**: alternative to hydration, serialize the state and "resume" where the server stopped, no re-execution client-side. Eg [Qwik](https://qwik.dev/docs/concepts/resumable/)

### ISR - Incremental Static Regeneration

- Derived from SSG: build-time generation + background regeneration after a delay or on-demand
- Static pages without a full rebuild
- Popularized by [Next.js](https://nextjs.org/docs/app/guides/incremental-static-regeneration): pass the `revalidate` option when calling `fetch`

Variant:

- **DPR / on-demand rendering**: static generation deferred to the first request, then cached; the page is generated once, deterministically, no stale content served. Eg [Netlify on-demand builders](https://docs.netlify.com/configure-builds/on-demand-builders/)

### Streaming SSR

- Derived from SSR: the server sends the HTML in chunks to display the ready parts fast
- Eg [React `<Suspense>`](https://react.dev/reference/react/Suspense) and [Next.js `loading.js`](https://nextjs.org/docs/app/api-reference/file-conventions/loading): stream HTML chunks as data becomes ready

### Edge rendering

- Derived from SSR: changes where the rendering happens, not how
- SSR executed on CDN nodes close to the user to reduce latency (TTFB)
- Eg [Vercel Functions](https://vercel.com/docs/functions), [Cloudflare Workers](https://developers.cloudflare.com/workers/)

### Server Components

- Components executed only on the server (zero client JS for them), mixed with client components
- Orthogonal to the page-level strategies: combines with SSG, ISR and SSR alike
- Eg [React Server Components](https://react.dev/reference/rsc/server-components), the [default in the Next.js App Router](https://nextjs.org/docs/app/getting-started/server-and-client-components)

## Summary

| Strategy | Rendered   | Data       | TTFB      | SEO  | Interactive     |
|----------|------------|------------|-----------|------|-----------------|
| CSR      | browser    | at runtime | fast      | weak | after JS        |
| SSR      | server     | fresh      | slower    | good | after hydration |
| SSG      | build      | frozen     | very fast | good | after hydration |
| ISR      | build + bg | semi-fresh | very fast | good | after hydration |

## How to choose

- Data update frequency
- Per-user personalization
- SEO requirements
- Level of interactivity
- Budget / infrastructure
- Site size (number of pages)

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
