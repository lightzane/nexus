# Nexus — root landing page

A standalone, build-free static page for the **root** of the `nexus` GitHub Pages
repository (`lightzane.github.io/nexus/`). It centers the brand graphic, plus a
single link to the template showcase (`./template/`) — anyone landing on
the bare root who isn't here for a specific portfolio still has somewhere to go.
This is _not_ part of
[`nexus-alpha`](https://github.com/lightzane/nexus-alpha)'s Vite app and is never
touched by `pnpm build`; it's deployed by hand to a separate repository.

## Files

- `index.html` — the page itself.
- `og-image-nexus.svg` — brand mark + "Nexus" wordmark. Inlined directly into
  `index.html` (no `<img>`, so the page renders with zero extra requests); kept
  here standalone too, as the export source for `og-image-nexus.jpg`. Copied from
  `nexus-alpha/public/og-image-template.svg`, which is the reusable template — swap
  the wordmark/tagline text there to brand a new sub (e.g. another
  `lightzane.github.io/nexus/<sub>` entry) the same way (and re-inline it into
  this `index.html` if you reuse this file for that sub's own landing page).
- `og-image-nexus.jpg` — rasterized version for `og:image`/`twitter:image` (
  generated — export it from `og-image-nexus.svg` via the `/svg-to-img` dev
  route in `nexus-alpha`, same as the JP portfolio's OG image).
- `favicon.svg` — copy of `nexus-alpha/public/favicon.svg`.

> **Keep these three in sync** — there is no build step here to dedupe them, so the
> brand graphic exists as three hand-maintained copies: `nexus-alpha/public/og-image-template.svg`
> (template), `nexus/og-image-nexus.svg` (standalone export source), and the inline
> `<svg>` inside `index.html` (the page's actual visual). Any layout/content/style
> change to one (e.g. text, gradient, glow) must be copied into the other two.
> Remove comments.

`index.html` also shows a hardcoded `"Nexus vX.Y.Z"` tag (plain HTML, deliberately outside
the SVG above so it never ends up baked into the OG image export) — this page is
build-free, so there's no `__APP_VERSION__` injection here like in `nexus-alpha`. Bump it
by hand to match `nexus-alpha/package.json`'s `version` whenever that changes.

## Deploying

1. In [`nexus-alpha`](https://github.com/lightzane/nexus-alpha), run `pnpm build` (base is `/nexus/jp/`) and copy the contents
   of `dist/` into a `jp/` folder at the root of the separate `nexus` repo.
2. On `main` (base is `/nexus/template/`), run `pnpm build` too and copy `dist/`
   into a `template/` folder the same way — this is the template showcase (its
   own sample projects, tech stack, and profile content) this page's link
   points at.
3. Export `og-image-nexus.jpg` from `og-image-nexus.svg` (1200×630, OG Image preset)
   and place it next to this `index.html`.
4. Copy `index.html`, `og-image-nexus.svg`, `og-image-nexus.jpg`, and `favicon.svg`
   from this folder to the root of the `nexus` repo.

Resulting `nexus` repo structure:

```
nexus/                     → lightzane.github.io/nexus/
├─ index.html               (this hub landing page)
├─ og-image-nexus.svg
├─ og-image-nexus.jpg
├─ favicon.svg
├─ jp/                      → lightzane.github.io/nexus/jp/
│  ├─ index.html             (nexus-alpha's `jp`-branch dist/ output)
│  └─ assets/...
└─ template/                → lightzane.github.io/nexus/template/
   ├─ index.html             (nexus-alpha's `main`-branch dist/ output)
   └─ assets/...
```
