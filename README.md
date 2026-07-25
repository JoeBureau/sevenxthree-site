# sevenxthree-site

Public marketing site for **sevenxthree.com**, served by GitHub Pages directly
from `main`. Deliberately minimal for now: a single static landing page, no build
step, no dependencies, no JavaScript.

This repo is **separate from the `speech-platform` monorepo** and from the iOS
app repo. Nothing here is deployed by `infra/deploy/deploy_changed.sh`.

## Layout

| Path          | Purpose |
| ------------- | ------- |
| `index.html`  | The landing page. Self-contained — inline CSS, no external requests. |
| `404.html`    | Not-found page, served automatically by GitHub Pages. |
| `favicon.svg` | Site mark. Waveform glyph on the shared sevenXthree brand gradient. |
| `CNAME`       | Custom domain (`sevenxthree.com`). **Do not delete** — removing it unbinds the domain from Pages. |
| `robots.txt`  | Allows all crawlers, points to the sitemap. |
| `sitemap.xml` | Single-URL sitemap. Update `lastmod` when the page changes materially. |
| `.nojekyll`   | Skips Jekyll processing; files are published verbatim. |

## Brand

The gradient is shared with the platform's app favicons
(`apps/{studio,editor,ops}/public/favicon.svg` in the monorepo):

- `#1f6fb2` → `#2bb3b3` (55%) → `#5fb83a`

Page background is `#0d1420`.

## Local preview

No toolchain required — open the file, or serve it so root-relative paths
(`/favicon.svg`) resolve the way they do in production:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying

Pages publishes from the **root of `main`**. Per the platform's git rules, real
content work goes through a PR rather than a direct push to the default branch:

```sh
git switch -c update-landing-copy
# ...edit...
git commit -am "site: update landing copy"
git push -u origin update-landing-copy
gh pr create
```

Merging the PR publishes within about a minute.

## DNS

Managed in Cloudflare. The apex must point at GitHub Pages' four `A` records
(plus `AAAA` if you want IPv6), with `www` as a `CNAME` to
`joebureau.github.io`. See GitHub's [apex domain
docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)
for the current addresses.

If the records are proxied through Cloudflare (orange cloud), set SSL/TLS mode to
**Full** — *Flexible* causes a redirect loop with Pages. Leaving the records
DNS-only (grey cloud) is the simplest path and lets GitHub issue its own
certificate.
