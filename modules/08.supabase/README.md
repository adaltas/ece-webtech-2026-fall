---
duration: 1h
---

# Storage and databases | with Supabase

## The problem: the application has no memory

- A frontend alone forgets everything: data hardcoded in the bundle, forms that submit to nowhere
- Redeploy, restart, reload: nothing survives, and two users never see the same thing
- Component state (module 4) has a bigger brother: **persistent data** - shared by all users, surviving deployments
- It lives in a database. The real question of this class: who is allowed to touch it, and where is that enforced?

## Databases, a refresher

- Organized collections of data: store, access, manage - not a one-type-fits-all scenario
- Relational (SQL): PostgreSQL, MySQL, SQLite - tables, relations, transactions, 50 years of theory
- NoSQL families: documents (MongoDB), key/value (Redis), column families (Cassandra), graphs (Neo4j), files and objects (POSIX, S3)
- For structured application data, relational won: SQL is the common tongue, and portable across providers

## PostgreSQL

- Open-source object-relational database, in active development since 1986
- Pronounced POST-gress, the QL is silent
- Feature-packed: JSON support, full-text search, geospatial, stored procedures in 12 languages
- Roles, privileges, schemas, and **row-level security**
- The [most popular database among developers](https://survey.stackoverflow.co/2024/technology#1-databases), and the substrate of a whole product generation: Supabase, Neon, and even Firebase added a Postgres offering

## Who talks to the database?

- The browser never speaks SQL to a database: something sits in between
- The module 3 way: a custom backend - Express routes, validation, authorization, an ORM. Full ownership, full maintenance
- The BaaS bet: that middle layer is generic enough to be generated and operated by a provider

## Backend as a Service

What it brings:

- A managed backend: database, auto-generated APIs, authentication, file storage, realtime
- The developer writes the frontend and the **access rules**, the provider runs the rest
- Speed: a working backend in minutes, no server to patch, free tiers to start

What it costs:

- Vendor coupling: proprietary APIs and services make leaving harder than arriving
- Business logic has no obvious home ("no backend" is a slogan, not an architecture): policies, database functions, edge functions
- Mitigation: prefer open standards (SQL, Postgres) and open-source providers that can be self-hosted

## The BaaS landscape

- **Firebase** (Google, 2011): the pioneer, NoSQL document store - and a follower now: it added a Postgres-based offering (Data Connect) a decade later
- **Supabase** (2020): the open-source alternative - plain Postgres with generated APIs, self-hostable, the choice of this course
- **Appwrite**, **PocketBase** (a single Go binary on SQLite): open-source alternatives
- **Convex**: TypeScript-first reactive database, no SQL
- Database-only cousins: **Neon**, **Turso** - serverless Postgres/SQLite without the auth and storage suite
- The AI effect, again: app generators (Lovable, Bolt, v0) provision Supabase by default

## Supabase

- Plain PostgreSQL + an auto-generated REST API ([PostgREST](https://postgrest.org)), GraphQL, realtime subscriptions
- Authentication (OAuth - module 9), file storage, edge functions
- [Studio](https://supabase.com/docs/guides/local-development/cli/getting-started#access-your-projects-services): the admin UI; [CLI](https://supabase.com/docs/guides/local-development): the full stack in local Docker containers, migrations, seeds
- Managed ([supabase.com](https://supabase.com)) or self-hosted: same open-source code, the schema is plain SQL either way
- [Client libraries](https://supabase.com/docs/reference/javascript/introduction) for the browser and the server

## The security model: RLS is the backend

- Without a middleware, the browser holds an API key and talks (almost) directly to the database
- The publishable key (historically "anon key") ships to every visitor: it is public by design, so security **cannot** live in the client
- It lives in the database: **row-level security** filters every query, row by row, through SQL policies
- Policies replace the route guards otherwise written in Express

```sql
alter table articles enable row level security;

create policy "Articles are public."
  on articles for select using (true);

create policy "Authors update their own articles."
  on articles for update using (auth.uid() = author_id);
```

- `auth.uid()` identifies the connected user - everyone is anonymous until module 9 plugs authentication in
- Policies are flexible: user role, execution context (eg the current date), business logic across tables
- Rule of thumb: **enable RLS on every table exposed to the API**, no exception - a table without policies is safer than a table without RLS

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
