# sveltekit-0auth-supabase

SvelteKit app for creating an account and signing in with Supabase Auth. A signed-in person can edit a `profiles` row and upload an avatar to the `avatars` storage bucket.

The lockfile pins `@sveltejs/kit` 1.0.0-next.511, `svelte` 3.50.1, `vite` 3.1.6, and `@supabase/supabase-js` 1.35.7. `svelte.config.js` uses `@sveltejs/adapter-auto`. The package version is `0.0.1` and the package is marked private.

## Prerequisites

- Node.js 16.14 or newer. Locked `@sveltejs/kit` declares `"node": ">=16.14"`.
- npm. This repo includes `package-lock.json` (lockfileVersion 2).
- A Supabase project.

Create a `.env` file in the project root. `.gitignore` ignores `.env` and `.env.*`. The repo does not contain a `.env.example` file.

```sh
printf '%s\n' 'VITE_SUPABASE_URL="YOUR_SUPABASE_URL"' 'VITE_SUPABASE_ANON_KEY="YOUR_SUPABASE_KEY"' > .env
```

Replace the two values with the project URL and the anon key from the Supabase project settings. `src/lib/supabaseClient.js` reads `import.meta.env.VITE_SUPABASE_URL` and `import.meta.env.VITE_SUPABASE_ANON_KEY` and creates the client at import time. A missing URL throws `supabaseUrl is required`, and the page returns 500.

The app uses this Supabase data:

- Auth, through `supabase.auth.signIn`, `supabase.auth.signUp`, `supabase.auth.signOut`, `supabase.auth.user`, and `supabase.auth.onAuthStateChange`.
- A `profiles` table. Signup upserts `id`, `email`, and `password`. The profile form reads `username`, `website`, and `avatar_url`, and upserts `id`, `username`, `website`, `avatar_url`, and `updated_at`.
- A storage bucket named `avatars`.

In the Supabase dashboard, create a project, open the SQL Editor, and run the User Management Starter. That is the database setup recorded for this project. Copy the project URL and anon key into `.env` after it finishes.

## Install, build, run, and test

Install the locked dependency tree:

```sh
npm ci
```

Start the dev server:

```sh
npm run dev
```

`dev` runs `vite dev` and prints a local URL. To open a browser as well:

```sh
npm run dev -- --open
```

Build for production:

```sh
npm run build
```

`build` runs `vite build` with `@sveltejs/adapter-auto`. When the adapter cannot detect a supported production environment, it prints that message and the command still exits 0.

Serve that build locally:

```sh
npm run preview
```

Check formatting:

```sh
npm run lint
```

`lint` runs `prettier --plugin-search-dir . --check .`. On the current tree it exits 1 because existing source files differ from `.prettierrc`.

Rewrite files with the same Prettier settings:

```sh
npm run format
```

`package.json` has no `test` script. `npm test` exits 1 with `Missing script: "test"`.

## Usage

Open the URL printed by `npm run dev`.

`/` (`src/routes/+page.svelte`) sets the `user` store from `supabase.auth.user()` and listens for auth changes. On `SIGNED_IN` it stores `session.user`. Otherwise it stores `false`.

When that store value is falsy, `/` shows the magic link form in `src/lib/Auth.svelte`. Submit an email and the form calls `supabase.auth.signIn({ email })`. On success it alerts `Check your email for the login link!`. The signed-out page also includes a link labeled `Get a temporary email address here to test it out!`.

The same page always shows the email and password form in `src/lib/loginForm.svelte`. Submit calls `supabase.auth.signIn({ email, password })`. A returned user sends the browser to `/dashboard`. A failure is passed to `console.log`.

When the store holds a user, `/` shows the profile form in place of the magic link form. The password form stays on the page.

`/signup` (`src/routes/signup/+page.svelte`) shows `src/lib/signupForm.svelte` with the title `Sign Up`. Submit calls `supabase.auth.signUp({ email, password })`, upserts `id`, `email`, and `password` into `profiles`, and then sends the browser to `/dashboard`. An error from signup or the upsert is shown with `alert`.

`/dashboard` (`src/routes/dashboard/+page.svelte`) shows the heading `Welcome to the dashboard` and renders `src/lib/Profile.svelte`. The form shows Email (disabled, from `$user.email`), Name, and Website. Update upserts the profile. The avatar control uploads the chosen image to the `avatars` bucket under a random file name, then runs that same update. Sign Out calls `supabase.auth.signOut()`.

`/dashboard` does not load the auth listener. `/` is what writes `supabase.auth.user()` into the shared store. With no session that call returns null. After `/` has stored null, `/dashboard` returns 500 while rendering the email field: `Cannot read properties of null (reading 'email')`. A fresh server whose first request is `/dashboard` still renders the form, with an empty email, because the store still holds its initial value `false`.

## Layout

```text
src/app.html                      HTML shell
src/lib/supabaseClient.js         Supabase client from the two VITE_ variables
src/lib/sessionStore.js           Svelte store for the current user
src/lib/Auth.svelte               Email magic link form
src/lib/loginForm.svelte          Email and password login form
src/lib/signupForm.svelte         Email and password signup form
src/lib/Profile.svelte            Profile fields, update, and sign out
src/lib/Avatar.svelte             Avatar download and upload
src/routes/+page.svelte           /
src/routes/signup/+page.svelte    /signup
src/routes/dashboard/+page.svelte /dashboard
static/favicon.png
svelte.config.js                  adapter-auto
vite.config.js                    SvelteKit Vite plugin
package.json                      scripts listed above
```

## License

This repository has no LICENSE file, and `package.json` does not set a license field.
