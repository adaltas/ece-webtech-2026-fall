---
duration: 2h
objectives:
  - Explain the OAuth 2.0 and OpenID Connect authorization code flow
  - Add email and GitHub login to the project with Supabase Auth
  - Restrict data access to authenticated users with RLS
  - Maintain authentication across two environments, local Docker and supabase.com
---

# Lab: add authentication to your project

With OAuth and OpenID Connect, authentication is delegated: no account creation, no login endpoints, no password ever reaching your application. In this lab, your project gets real users - email and GitHub login through Supabase Auth, members-only data access - working both on your local Docker stack and on the deployed application.

## 1. Understand the flow

1. Watch [*An Illustrated Guide to OAuth and OpenID Connect*](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc) (16 minutes) to anchor the class concepts.
2. In a `docs/AUTH.md` file, map the OAuth actors to your own stack: who is the Resource Owner, the Client, the Authorization Server, the Resource Server? One line each - you will verify your answers against reality in part 3.

## 2. Email login with Supabase Auth

1. Follow the [server-side auth guide for Next.js](https://supabase.com/docs/guides/auth/server-side/nextjs): it replaces your single client module (lab 8) with a browser client, a server client, and the middleware keeping the session alive.
2. Build a `/login` page with a controlled form (lab 7) wired to [password-based auth](https://supabase.com/docs/guides/auth/passwords): sign up and sign in. The Supabase Auth UI component is deprecated - your form skills are the replacement.
3. In the header: show the logged-in user's email and a logout button ([`signOut`](https://supabase.com/docs/reference/javascript/auth-signout)), or a link to `/login` otherwise.

> [!NOTE]
> On the local stack, no real email leaves your machine: confirmation mails are caught by the local mail server listed in `supabase status`.

Verify: sign up, log out, log back in, reload - the session survives.

## 3. Login with GitHub

1. [Register an OAuth application on GitHub](https://github.com/settings/applications/new) for your **local** environment, with the callback URL of your local stack: `http://127.0.0.1:54321/auth/v1/callback`.
2. Enable the provider in `supabase/config.toml` and keep the secret out of Git with an `env()` reference, as shown in the [local development auth configuration](https://supabase.com/docs/guides/local-development/managing-config#auth). Restart the stack to apply.
3. Add a "Continue with GitHub" button to your login page calling [`signInWithOAuth`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth), with the callback route from the server-side auth guide.

Verify: the full round trip - your app → GitHub consent → back in your app, logged in. Compare what you observed (redirects, `code` parameter in the URL) with your `docs/AUTH.md` predictions and correct them.

## 4. Members only

Authentication exists to drive **authorization**. In a new migration, make at least one write operation of your project require a logged-in user - likes, comments, ratings, whatever fits - with an RLS policy based on `auth.uid()` (see the [RLS guide](https://supabase.com/docs/guides/database/postgres/row-level-security)). If the table records who wrote what, default the `user_id` column to `auth.uid()`.

Verify from the application: logged out, the write is rejected; logged in, it succeeds.

## 5. The two environments

Everything you built now exists twice, and the two copies are maintained differently. This is the daily reality of operating an application:

| Concern              | Local (Docker)                                | Production (supabase.com)                            |
|----------------------|-----------------------------------------------|------------------------------------------------------|
| Schema and policies  | migrations, `supabase db reset`               | `supabase db push`                                   |
| Auth provider config | `supabase/config.toml`, committed             | Dashboard > Authentication > Providers               |
| Secrets              | env file, ignored by Git                      | Dashboard fields, Vercel environment variables       |
| GitHub OAuth app     | callback `http://127.0.0.1:54321/auth/v1/callback` | a **second** GitHub app, callback `https://<project-ref>.supabase.co/auth/v1/callback` |
| Application URLs     | `http://localhost:3000`                       | [Site URL and redirect allow-list](https://supabase.com/docs/guides/auth/redirect-urls) |

Bring production up to date:

1. Push your new migrations (lab 8 workflow).
2. Register a second GitHub OAuth application for production and configure the provider in the dashboard, following the [GitHub login guide](https://supabase.com/docs/guides/auth/social-login/auth-github).
3. Set the Site URL to your Vercel domain and allow-list your redirect URLs (preview deployments included, the guide covers wildcards).

Verify on the deployed URL: email login, GitHub login, and the members-only write all work. Keep the table above in `docs/AUTH.md`: it is your checklist for every future auth change.

## 6. Bonus - one more provider

Add a second OAuth provider of your choice ([Google](https://supabase.com/docs/guides/auth/social-login/auth-google), Discord, GitLab, ...) - in both environments.

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
