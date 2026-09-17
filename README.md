# fernandoenterprise.com

The Fernando Enterprise company website: five pages of static HTML, one stylesheet,
three images. No build step, no framework, no JavaScript.

## What is here

```
index.html                        Home — what the company builds, the two apps, contact
apps/ca-hunt-fish-guide/          CA Hunt & Fish Guide product page
support/                          How to reach the company; app support routing
privacy/                          The SITE privacy statement (the apps have their own)
about/                            Antonio Esquivel, legal name, address, email
404.html                          Served by GitHub Pages for unknown paths
assets/site.css                   The only stylesheet
assets/app-icon-512.png           App icon, downscaled from the iOS 1024 source
assets/app-icon-192.png           App icon, card size
assets/favicon-32.png             Favicon, raster
assets/favicon.svg                Favicon, vector (the ridge-and-fish mark)
CNAME                             Must contain exactly: fernandoenterprise.com
.nojekyll                         Serve the files as-is; no Jekyll processing
robots.txt                        Allow all; points at the sitemap
sitemap.xml                       Absolute https://fernandoenterprise.com/ URLs
```

## Editing it

Open the file and edit the HTML. There is nothing to compile. To preview locally:

```sh
python3 -m http.server 8000     # then open http://localhost:8000
```

Root-relative links (`/support/`, `/assets/site.css`) are used throughout, so preview
from the repository root or the paths will not resolve.

### Two things to confirm before this goes live

Search the tree for `CONFIRM-LLC`:

```sh
grep -rn "CONFIRM-LLC" .
```

Both hits — the footer legal line (in every page) and the "Legal name" row on the About
page — print **Fernando Enterprise LLC**. That is the intended legal name. If the LLC is
not actually filed yet, change those two places to **Fernando Enterprise** and leave
everything else alone; no other page states an entity type.

### House rules for the copy

- No phone number anywhere.
- The food-truck compliance app has no settled name, so no name is printed for it.
- CA Hunt & Fish Guide is not on the App Store yet: the page says "coming to the App
  Store" and carries no store link. Add the link when the app is live.
- The unofficial / not-affiliated disclaimer stays on the product page. It is an App
  Store requirement as well as the truth.
- No third-party scripts, fonts, analytics or cookies. The privacy page says the site
  sets none, and that must stay true.

## Hosting: GitHub Pages

The site is a project site in the **Fernando-Enterprise-LLC** organisation.

1. Push this repository to the organisation.
2. Repository → **Settings** → **Pages**.
3. **Source**: "Deploy from a branch". **Branch**: `main`, folder `/ (root)`. Save.
4. **Custom domain**: enter `fernandoenterprise.com` and save. GitHub reads the `CNAME`
   file in the repository root; keep that file's contents exactly
   `fernandoenterprise.com` and do not delete it.
5. Wait for the DNS check to go green (see below — it will fail until the A records are
   changed), then tick **Enforce HTTPS**. The certificate is issued by GitHub and can
   take up to an hour after DNS resolves.

## DNS at Squarespace Domains

The domain is registered at Squarespace and currently points at Squarespace's parking
page. Google Workspace mail for `tonio@fernandoenterprise.com` runs on the same domain
and **must not be disturbed**.

### Do not touch

- **MX** records — including `1 smtp.google.com`. Mail breaks if these change.
- The **SPF** `TXT` record (the one starting `v=spf1`). Mail starts going to spam if this
  changes.
- Any `TXT` verification record, and any DKIM/`_domainkey` record if one is present.

### Replace: A records for the apex (`@`)

Delete the four Squarespace parking addresses:

```
198.185.159.144
198.185.159.145
198.49.23.144
198.49.23.145
```

Add the four GitHub Pages addresses, all as `A` records on the apex (`@`):

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

### Optional: AAAA records for the apex (`@`)

IPv6, not required. Add all four if you add any:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

### Replace: CNAME for `www`

Change the `www` record from `ext-sq.squarespace.com` to:

```
fernando-enterprise-llc.github.io
```

(No trailing dot needed in the Squarespace editor; it adds one itself.)

### Checking it

DNS changes can take up to a few hours to propagate. To watch:

```sh
dig +short fernandoenterprise.com A
dig +short www.fernandoenterprise.com CNAME
dig +short fernandoenterprise.com MX          # must still show smtp.google.com
```

The apex should return the four `185.199.*.153` addresses, `www` should return
`fernando-enterprise-llc.github.io`, and the MX record must be unchanged. When that is
true, GitHub's Pages settings page will verify the domain and let you enforce HTTPS.

### A note on Squarespace

Squarespace prompted for a website when the domain was set up for email. It is not
needed: the domain is used for Google Workspace mail and for this site on GitHub Pages.
No Squarespace site has to be built, published, or paid for. Only the DNS records above
are managed there.
