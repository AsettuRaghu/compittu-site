# compittu-site

The customer-facing marketing site for **Compittu**, a compliance / risk / governance
platform sold to hospitals, schools, hotels, restaurants, manufacturing, retail and
logistics businesses.

This repo is **only the marketing site**. The product is a separate, private monorepo at
`~/Projects/compittu` (Next.js + NestJS + Prisma/Neon). The two are deliberately
decoupled — separate repos, separate deploys, separate editor windows. Don't propose
merging them or sharing packages between them.

## Hosting

- Static Astro build → **GitHub Pages**, live at <https://compittu.com>
- Repo: `github.com/AsettuRaghu/compittu-site`, public (Pages on a private repo needs a paid plan)
- Push to `main` deploys via `.github/workflows/deploy.yml` (`withastro/action@v3`), ~90s
- Domain registered at Squarespace; Squarespace DNS points at GitHub's four `A` records
  plus `www` CNAME → `asetturaghu.github.io`
- `public/CNAME` pins the custom domain — **do not delete it**, or deploys drop the domain
- The product app is expected to live at `app.compittu.com` later; the site will link to it

## Gotchas that already bit us

- **Design tokens are copied, not imported.** `src/styles/global.css` mirrors
  `--color-primary` and the radius from `compittu/apps/web/src/app/globals.css`. If the
  product theme changes, update this file by hand — there is no shared package.
- **`minimumReleaseAge: 1440` in `pnpm-workspace.yaml` is deliberate.** CI's pnpm rejects
  dependencies published within 24h; local pnpm 10.20 doesn't enforce it by default, so
  without this setting the lockfile can pin a just-published version and fail CI with
  `ERR_PNPM_MINIMUM_RELEASE_AGE_VIOLATION`. If that happens, loosen the dependency range
  and re-resolve — **do not disable the policy.**
- **`pnpm clean --lockfile` does not exist** in pnpm 10.20, despite the error message
  recommending it. Delete `pnpm-lock.yaml` and reinstall to force re-resolution.
- **The same Squarespace DNS carries live Resend email records** (`MX send`, `TXT send`
  SPF, `resend._domainkey` DKIM, `_dmarc`) on the `send` subdomain, not the apex. Never
  touch them when changing web DNS.
- `CLAUDE.md` is a symlink to this file. Edit `AGENTS.md`.

## Known TODO

- The waitlist form in `src/pages/index.astro` posts to `FORMSPREE_ENDPOINT`, still set to
  the placeholder `https://formspree.io/f/REPLACE_ME`. Submissions fail until a real
  Formspree form ID replaces it.

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
