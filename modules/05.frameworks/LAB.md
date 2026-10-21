---
duration: 2h
objectives:
  - Install Next.js manually
  - Design an application in terms of pages, layouts and components
  - Create every page with a shared layout, navigation and a custom 404
  - Deploy to Vercel with continuous previews
---

# Lab: initialize your project with Next.js

The Tic-Tac-Toe was a training ground. Starting now, you build your course project: a web application **of your choice** - a blog is the classic example, used throughout the course material - that you will carry through the rest of the course (rendering, database, authentication, deployment). This lab is about the skeleton: no real content, no styling - only pages, layout and navigation, done right.

## Prerequisites

Once the previous lab is tagged (`lab4`), you may remove the `tic-tac-toe` folder from your repository: the tag keeps it accessible.

## 1. Clean up and install Next.js manually

No `create-next-app` here: install Next.js by hand to see that a framework is, at the bottom, just packages and conventions. Follow the [manual installation](https://nextjs.org/docs/app/getting-started/installation#manual-installation) section of the official documentation (JavaScript, App Router, latest version).

1. Under your repository root, create an `app` folder for the project and install Next.js in it.

   > Yes, you end up with `app/app/`: the outer folder is your project, the inner one is the App Router convention.

2. Follow the manual installation steps until `npm run dev` serves a page on [http://localhost:3000](http://localhost:3000).
3. Make sure `node_modules` and `.next` are ignored by Git.
4. Commit - as for every part below.

## 2. Design your application before coding it

Pick your project, then think it in the terms of the class: pages, layouts, components. Write the result in a `docs/ARCHITECTURE.md` file **before writing any code**:

1. The list of routes, including at least one collection with its dynamic route. For example, for a blog:

   | Route            | Content                    | Static or dynamic route? |
   |------------------|----------------------------|--------------------------|
   | `/`              | home                       | ?                        |
   | `/about`         | about the site             | ?                        |
   | `/contacts`      | contact information        | ?                        |
   | `/articles`      | list of articles           | ?                        |
   | `/articles/:???` | one article                | ?                        |

   Adapt to your project (a recipe site has recipes, a game library has games, ...). Pick your URL parameter (`:slug`, `:id`, ...) and justify it.

2. What belongs to the layout (shared by every page) and what belongs to each page.
3. The list of your own components (`Header`, `Footer`, `Nav`, ...).

## 3. Create your pages and layout

Read the three pages of the official documentation - they are short and cover everything needed:

1. [Project structure](https://nextjs.org/docs/app/getting-started/project-structure)
2. [Layouts and pages](https://nextjs.org/docs/app/getting-started/layouts-and-pages)
3. [Linking and navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating)

Then implement your `ARCHITECTURE.md`:

1. Create every static route as a placeholder page: a title and a line of dummy text, nothing more. Content and styling come in later modules.
2. Create the `Header`, `Footer` and `Nav` components in a `components/` folder and assemble them in the root layout.
3. The navigation menu must link to every page with the `Link` component.

Verify: navigating through the menu switches pages without a full page reload (no spinner in the browser tab), and the layout stays in place.

## 4. Dynamic routes and error handling

1. Create the dynamic route for a single item of your collection (eg one article), displaying its URL parameter as placeholder content. Hardcode a small list of known items in a module (eg `lib/articles.js`) and display them as links on the collection page.
2. When the URL parameter doesn't match any known item, return the 404 page with [`notFound()`](https://nextjs.org/docs/app/api-reference/functions/not-found).
3. Customize the 404 page with a [`not-found.js`](https://nextjs.org/docs/app/api-reference/file-conventions/not-found) file - make it link back to the home page.

Verify: the page of an existing item renders, an unknown item and `/nonexistent` both show your custom 404.

## 5. Deploy to Vercel

Put your skeleton online now: from this point on, every push deploys, and you (and your teacher) can see a live preview of the project during the whole semester.

1. Make sure a production build passes locally: `npm run build && npm run start`.
2. Sign up on [Vercel](https://vercel.com) with your GitHub account and create a new project by importing your repository. Set the **Root Directory** to your application folder (`app`), keep the detected Next.js defaults.
3. Open the deployed URL and navigate your pages, including the 404.

From now on, every push to your main branch triggers a production deployment, and every branch or pull request gets its own [preview URL](https://vercel.com/docs/deployments/environments#preview-environment-pre-production). End every future lab by checking your deployment.

> [!WARNING]
> On the free (Hobby) plan, only commits authored by the GitHub account connected to the Vercel project - its owner - trigger a deployment. Commits from other contributors are skipped. See [Why aren't commits triggering deployments on Vercel?](https://vercel.com/guides/why-aren-t-commits-triggering-deployments-on-vercel)

## 6. Bonus - highlight the active page

Highlight the current page in the navigation menu using the [`usePathname`](https://nextjs.org/docs/app/api-reference/functions/use-pathname) hook. It requires turning `Nav` into a Client Component (`'use client'`) - module 6 explains what that implies.

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
