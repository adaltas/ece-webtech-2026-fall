---
duration: 2h
objectives:
  - Run Supabase locally with the CLI and Docker
  - Model project data with SQL migrations and seeds
  - Query Postgres from the right components, secured by row-level security
  - Provision a hosted Supabase and keep it in sync with migrations
---

# Lab: Supabase as your backend

Your items are hardcoded in a module and your contact form logs to the console. Time for a real backend: a local Supabase running in Docker for development - fully offline - with the schema versioned as migrations in Git, and a hosted project on supabase.com behind your Vercel deployment.

## Prerequisites

A container runtime ([Docker Desktop](https://docs.docker.com/desktop/) or an [alternative](https://supabase.com/docs/guides/local-development/cli/getting-started#running-supabase-locally)) must be installed and running.

## 1. Run Supabase locally

Follow the [CLI getting started guide](https://supabase.com/docs/guides/local-development/cli/getting-started): install the CLI, run `supabase init` at your repository root (the generated `supabase/` folder is committed), then `supabase start`.

Verify:

- `supabase status` prints the local URLs and keys
- [Supabase Studio](http://localhost:54323) opens and shows an empty database

Everything runs on your machine: no account, no network needed from here until part 5.

## 2. Model your data with migrations

No click-only schema: your tables are **code, versioned in Git**, so any teammate (or your future self) can rebuild the database from scratch.

1. Design the tables for your project data, consistent with your `docs/ARCHITECTURE.md`. For a blog: an `articles` table (unique `slug`, `title`, `content`, `created_at`) and a `contacts` table matching the fields of your lab 7 form.
2. Write them as [migrations](https://supabase.com/docs/guides/deployment/database-migrations): `supabase migration new <name>`, SQL inside, `supabase db reset` to apply. You can also experiment in Studio first and capture the result with `supabase db diff -f <name>`.
3. [Seed](https://supabase.com/docs/guides/local-development/seeding-your-database) dummy rows in `supabase/seed.sql` - migrate the content of your hardcoded module there.

Verify: `supabase db reset` rebuilds the whole database (schema + seed) without an error, and the rows show in Studio.

## 3. Plug Next.js on Postgres

1. Install [`@supabase/supabase-js`](https://supabase.com/docs/reference/javascript/initializing) in your application and create a single client module (eg `lib/supabase.js`). Configure `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` in `app/.env.local` with the values printed by `supabase status` - and never commit this file.
2. Replace your hardcoded data: the collection page and the item page now [select](https://supabase.com/docs/reference/javascript/select) from the database. Keep the `notFound()` behavior for unknown items.
3. Wire the lab 7 contact form: on submit, [insert](https://supabase.com/docs/reference/javascript/insert) into your `contacts` table. Keep the validation and success states.
4. Revisit `docs/RENDERING.md` (lab 6): a page baked at build time does not see new rows. Adjust the strategies (`revalidate`, `generateStaticParams`, ...) if your decisions no longer hold, and check with `npm run build`.

> [!NOTE]
> The data queries stay in server components, the contact form stays a client component - the module 6 boundary doesn't move. Module 9 introduces [`@supabase/ssr`](https://supabase.com/docs/guides/auth/server-side/nextjs) for authenticated requests; the plain client is enough for now.

## 4. Row-level security

Read the [row-level security guide](https://supabase.com/docs/guides/database/postgres/row-level-security). Your publishable key ships to every visitor: the policies are the only thing standing between the Internet and your tables.

In a new migration:

1. Enable RLS on **every** table.
2. Write the policies implementing your rules. For the blog example: articles are readable by everyone and writable by no one; contacts can be inserted by everyone and read by no one.

Verify from the application, not from Studio (Studio queries bypass RLS):

- The pages still render and the form still inserts
- A `select` on `contacts` from the application returns no rows
- Drop one policy (`supabase db reset` after editing): the matching feature must break

## 5. Provision supabase.com

Your Vercel deployment still has no database behind it. Supabase hosts [small projects for free](https://supabase.com/pricing).

1. Create a project on [supabase.com](https://supabase.com/dashboard), then [link and push your migrations](https://supabase.com/docs/guides/deployment/database-migrations#deploy-a-migration): `supabase login`, `supabase link`, `supabase db push`. Your hosted schema is now exactly the one in Git - seed a few rows from the dashboard SQL editor if needed.
2. In the Vercel project settings, set `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` to the hosted project values ([API settings](https://supabase.com/dashboard/project/_/settings/api-keys)), and redeploy.
3. Verify on the deployed URL: data renders, the contact form inserts, and RLS still applies.

This is your workflow from now on: develop offline against the local stack, commit migrations, `db push` when ready.

## 6. Bonus - a full feature, end to end

Add one data feature to your project - comments on articles, likes, ratings, whatever fits. The full chain: a migration (table + RLS policies), a styled stateful component (lab 7), the right rendering strategy (lab 6), pushed to production. Optional: make it live with a [realtime subscription](https://supabase.com/docs/guides/realtime).

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
