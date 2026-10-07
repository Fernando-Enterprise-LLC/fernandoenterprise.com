# fernandoenterprise.com

The Fernando Enterprise company website: static HTML and one stylesheet, served by GitHub
Pages from the `main` branch. No build step, no framework, no JavaScript.

## What is here

```
index.html                        Home
about/                            The company, its legal name, address and email
apps/ca-hunt-fish-guide/          CA Hunt & Fish Guide product page
support/                          How to reach the company
privacy/                          The website privacy statement (the apps have their own)
accessibility/                    The accessibility statement
404.html                          Served for unknown paths
assets/                           Stylesheet, icons, and the link-preview image
favicon.ico, apple-touch-icon.png Icons browsers and phones request at the root
.well-known/security.txt          How to report a security issue
CNAME                             The custom domain: fernandoenterprise.com
.nojekyll                         Serve the files as they are
robots.txt, sitemap.xml           For search engines
```

## Editing it

Edit the HTML directly. To preview locally, run this from the repository root:

```sh
python3 -m http.server 8000
```

then open the address it prints. Links are root-relative (`/support/`), so preview from the
repository root.

Each page carries a canonical link and link-preview tags (`og:*`, `twitter:card`) in its
head; a new page needs the same block. Add every new page to `sitemap.xml`.
