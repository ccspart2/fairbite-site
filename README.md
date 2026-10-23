# fairbite-site

The public website for **FairBite**, an Android app that splits a restaurant bill fairly,
to the cent. It is the app's home page, its privacy policy (the URL given to Google Play)
and its support page.

This repository holds **site content only**. The app itself lives elsewhere and is not
public.

- Hosting: GitHub Pages, deployed from `main`, root folder.
- Plain static HTML and one stylesheet. No build step, no frameworks, no external
  scripts, fonts or CDNs, no analytics, no cookies.
- The only JavaScript is a three-line inline script in `404.html` (see below).

## What's here

```
index.html            Home: hero, how it works, features, screenshots, pricing
privacy/index.html    Privacy policy  → /privacy/
support/index.html    Support: contact and FAQ  → /support/
404.html              Not-found page (GitHub Pages serves it for any missing path)
assets/css/site.css   The one stylesheet (the app's dark colour scheme and plate colours)
assets/img/           Logo, PNG icons, Open Graph image (1200×630)
assets/screenshots/   App screenshots, WebP, 1600 px and 800 px wide
fonts/                Plus Jakarta Sans (WOFF2) and its licence, OFL.txt
favicon.svg           The app icon as an SVG
robots.txt, sitemap.xml, .nojekyll
LICENSE               Content licence (all rights reserved; the font is under the OFL)
```

Every internal link is **relative**, so the site works both under the project path
(`https://ccspart2.github.io/fairbite-site/`) and at the root of a custom domain.

## Preview locally

Serve it from a parent folder so you also test the `/fairbite-site/` subpath:

```bash
cd ..                      # the folder that contains fairbite-site/
python3 -m http.server 8000
# open http://localhost:8000/fairbite-site/
```

Or, to preview it as it would look at a domain root:

```bash
python3 -m http.server 8000   # from inside fairbite-site/
# open http://localhost:8000/
```

Opening the HTML files directly (`file://`) mostly works, but folder links such as
`support/` open a directory listing instead of `index.html`; use a server.

## Switching the Google Play badge on

Until FairBite is live, the home page shows a **"Coming soon on Google Play"** box.
At launch:

1. In `index.html`, find the block marked `GOOGLE PLAY BADGE` (in the hero).
   Delete the `<p class="coming-soon">` element and uncomment the
   `<a class="store-badge">` block below it. Check the store link
   (`https://play.google.com/store/apps/details?id=com.fairbite.app`).
2. Download the official **"Get it on Google Play"** badge from Google's badge page
   (play.google.com/intl/en_us/badges) and save it as
   `assets/img/google-play-badge.png`. Don't redraw or recolour it; adjust the `width`
   and `height` attributes to its real proportions.
3. In the footer of **every** page (`index.html`, `privacy/index.html`,
   `support/index.html`, `404.html`), uncomment the line
   *"Google Play and the Google Play logo are trademarks of Google LLC."*
4. Optionally change the hero's `store-note` sentence.

## Changing the base URL (custom domain)

Relative links don't care about the domain. Only the **absolute** URLs do: the
`canonical`, Open Graph and Twitter tags in each page's `<head>`, `sitemap.xml` and
`robots.txt`. They all use exactly this string:

```
https://ccspart2.github.io/fairbite-site/
```

To move to a custom domain (say `https://fairbite.app/`), replace it everywhere:

```bash
grep -rl 'https://ccspart2.github.io/fairbite-site/' . --include='*.html' --include='*.xml' --include='*.txt' \
  | xargs sed -i '' 's#https://ccspart2.github.io/fairbite-site/#https://fairbite.app/#g'
```

(On Linux, use `sed -i` without the `''`.) Then:

1. Add a `CNAME` file at the root containing just the domain (`fairbite.app`).
2. Set up DNS for GitHub Pages, then in **Settings → Pages** enter the domain and turn on
   **Enforce HTTPS**.
3. `404.html` works out its own base: under `/fairbite-site/` it uses that path, and
   anywhere else it uses `/`. If the project path ever changes, edit the `sub` value in its
   inline script.

Decide the domain **before** the privacy policy URL goes into Play Console, so that URL
never has to change.

Note: crawlers only read `robots.txt` at a domain root. Under the project path it has no
effect (the sitemap can still be submitted directly); with a custom domain it starts
working.

## Adding Spanish later

The site is English only for now, with `lang="en"` on every page. Spanish (es-419) is
planned as a mirror under `/es/` (`es/index.html`, `es/privacy/`, `es/support/`), with
`lang="es-419"`, `../` added to every relative path, `hreflang` alternate links between
the two languages, a language switch in the header, and the new URLs in `sitemap.xml`.

## Content rules

- The privacy policy text is the approved wording; change it only together with the
  app's Play Data safety answers, and update its effective date.
- Never commit anything from the app repository other than marketing content:
  no source, specs, issues, keys, `local.properties`, keystores or order details.
- Screenshots come from the debug app on an emulator, with made-up example bills.
