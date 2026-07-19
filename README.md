# Sherman Nagel Adventist High School Website

Public website for Sherman Nagel Adventist High School, Owerri. The site is
plain HTML, CSS and JavaScript with no build step.

## Project structure

```text
client/
  index.html   Homepage
  404.html     Custom not-found page
  _headers     Cloudflare security and cache headers
wrangler.toml  Cloudflare Workers static-assets configuration
```

CSS, JavaScript, branding and replaceable photography are served from the
separate `snahs-assets` repository at `assets.shermannagel.sch.ng`. Public forms
submit to the existing portal backend at `api.shermannagel.sch.ng`.

Replace photography in `snahs-assets/public/website/images/` using the existing
filenames. Shared branding and browser/PWA icons live in
`snahs-assets/public/shared/images/`; do not add local copies under `client/`.

Unknown school content is intentionally written as `[BRACKETED PLACEHOLDERS]`
so it can be located and replaced with a repository-wide search.

## Preview locally

From the repository root:

```powershell
npx wrangler dev
```

Wrangler prints the local preview URL, normally `http://localhost:8787`.

## Deploy to Cloudflare

```powershell
npx wrangler deploy
```

For Cloudflare Workers Builds, connect the GitHub repository and set the deploy
command to `npx wrangler deploy`. No build command or output directory is
required because `wrangler.toml` publishes `client/` directly.

Deploy `snahs-assets` first whenever a shared file or asset URL changes.

After the first deployment, add `www.shermannagel.sch.ng` as the Worker's custom
domain in Cloudflare, then add the apex `shermannagel.sch.ng` domain.

Before enabling forms in production:

1. Add both website origins to the API backend's `WEBSITE_URL` environment variable.
2. Run `database/migration-2026-07-19-website-forms.sql` from the portal repository.
3. Replace `[CLOUDFLARE_TURNSTILE_SITE_KEY]` and set the API's matching
   `TURNSTILE_SECRET_KEY`, or leave both unset to run without Turnstile.
