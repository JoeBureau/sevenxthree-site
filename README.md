# sevenxthree-site

Public marketing site for **sevenxthree.com**, served by GitHub Pages directly
from `main`. Still deliberately minimal — a single static page, no build step, no
dependencies, no JavaScript — but no longer a holding page: since 2026-09-04 it
carries the capability pitch and the account-creation call to action.

This repo is **separate from the `speech-platform` monorepo** and from the iOS
app repo. Nothing here is deployed by `infra/deploy/deploy_changed.sh`.

## Why this page matters more than it looks

The free tier was retired on 2026-09-04 (`docs/pricing.md` in the monorepo), and
the phone apps may carry **no signup, no link and no plan copy** — an in-app route
to a signup that leads to a subscription is prohibited steering under Google
Play's Payments policy, and a "call to action for purchase" under App Store
3.1.3(f). Nobody can become a customer by browsing an app store either, because a
stranger who installs the app cannot use it.

**So this page is the entire top of the funnel, and the only one.** Two
consequences for anyone editing it:

- **The signup call to action is load-bearing.** If it breaks, there is no other
  way into the product. It points at `studio.sevenxthree.com/register`.
- **Prices are not here yet, and that is deliberate twice over.** Connect's price
  is undecided (`pricing.md` §11 q.5), and keeping the commercial surface on its
  own page rather than the one the apps name is the safer reading of both stores'
  rules. When plans are published they get their own page.

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
