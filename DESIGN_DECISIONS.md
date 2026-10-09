# CodeShift website: design decisions

Public address: https://css.mygig.life. Hosted on GitHub Pages, DNS on Cloudflare.

## What the site is

A single-page product site for two separate Android apps, plus one small guide page:

| Path | Purpose |
|---|---|
| `/` | Hero, CodeShift Keyboard (with the Everyday/Code switch), CodeShift Terminal, "two apps, one idea", privacy, availability, FAQ |
| `/privacy/` | A short guide to which privacy document covers which app. It links to the existing policies and does not copy them |
| `/404.html` | Not-found page |

The second app is named **CodeShift Terminal** on Google Play and in its own listing. It is an SSH terminal, SFTP file manager and code editor. The brief calls it "SSH Editor"; the site uses the real product name.

## Where the facts come from

Every claim comes from one of: the Play store listings in `docs/store/` of the app repository, the in-app privacy page (`PrivacyPage.kt`, which has a test that fails if the internet permission is ever added), the manifests, or the audited source. Planned features are not advertised. Specifically:

- Status is "closed testing on Google Play" for both apps, because that is the real state. No download buttons, no waiting list, no release dates.
- Prices are not stated. Both listings say one-time purchase with no subscriptions or in-app purchases; the amounts are set at release.
- Voice typing is described as handled by Google or Android's speech service, as the keyboard's own privacy page says.
- Not claimed: end-to-end encryption, "nothing is stored", "no internet" for the terminal (it needs the network to reach your servers).

## Identity

Idea: tools that remove friction. The memorable element is the Everyday/Code switch on the keyboard section, built from real emulator captures of the same keyboard in both modes. Everything else is deliberately quiet.

### Colour

| Name | Value | Use |
|---|---|---|
| Paper | `#f4f5f7` | Page background. Cool grey, not cream |
| Ink | `#0f1320` | Text and the switch |
| Slate | `#4b5366` | Secondary text (7.1:1 on paper) |
| Cobalt | `#2457e6` / `#1a43b8` | Links and focus. Taken from the apps' own blue |
| Night | `#0e1016` | The Terminal section, matching the terminal's own background |
| Line | `#d3d7df` | Hairlines |

The light page lets the apps' dark interfaces carry the contrast. The dark band is used once, for the terminal. No gradients, no glow.

### Type

- **Schibsted Grotesk** (400, 600, 800) for everything. Heavy, tightly tracked headlines; plain body at 1.0625rem/1.6 with lines capped near 62 characters.
- **JetBrains Mono** (400) only for genuine technical text: key labels like `</>` and `ABC`, and file paths.
- Both are SIL Open Font License, self-hosted as woff2 in `fonts/`, with `font-display: swap` and the two main weights preloaded.

### Composition

Left-aligned throughout, ruled lists rather than cards, and a different composition for each section so the two products do not look alike:

- Hero: headline and two product entries on the left, two overlapping real screenshots on the right.
- Keyboard: a switch above one phone and the explanation beside it; then two alternating text/phone bands; then a horizontal strip of small captures.
- Terminal: a dark full-width band, four short columns by task (connect, work, edit, transfer), two phones, and a strip.
- Privacy: one large statement and two plain fact lists.

No numbered markers (nothing here is a sequence), no all-caps labels, no icon grid, no testimonials or counts.

### Motion

One thing moves: the screenshot cross-fade when the switch changes. It is off when the visitor asks for reduced motion.

## The Everyday/Code switch

Pure CSS: two radio buttons with labels, styled as a segmented control. No JavaScript, so it works on touch screens, with a keyboard (arrow keys move between options, with a visible focus ring) and with screen readers (it is a real fieldset and radio group). It does not open a text field, so it never raises the visitor's own on-screen keyboard.

## Privacy of the website itself

- No cookies, analytics, pixels, fingerprinting or third-party scripts. There is no JavaScript at all.
- No external requests: fonts, images and styles are all served from this site. Verified in a browser at six widths on every page.
- A strict Content Security Policy in a `<meta>` tag: `default-src 'none'` with only same-origin images, styles and fonts allowed. It also means inline `style` attributes and scripts are blocked, so none are used.
- No forms and no email collection. Contact is a `mailto:` link to the existing app.cs@mygig.life.
- The site is on GitHub Pages, which keeps its own server logs. The page says so.

## Copy

A full copy review on 2026-10-09 was applied: plain developer voice, complete sentences, product names instead of vague "it", American English, and no marketing filler. Where a suggested rewrite would have made a privacy claim less accurate, the accurate wording was kept: the keyboard asks for the microphone permission only if you use its built-in voice engine; clipboard history stays on the device until it expires or you clear it (it is saved, so "not saved permanently" would be wrong); SSH traffic goes straight to your server or through a jump host you configure.

## Legal pages

The existing documents stay exactly where they are, served by the repository `myGIGlife-claude/app-codeshift` at `codeshift.mygig.life`:

- `/privacy/` keyboard privacy policy
- `/terminal/privacy/` terminal privacy policy
- `/terms/` keyboard terms of use

That address is the privacy-policy URL registered in the Play listings for both apps, so it must not move or redirect. The new site links to those exact URLs (footer, privacy section and the `/privacy/` guide) and does not copy their text.

Gap: there is no terms-of-use document for CodeShift Terminal. The guide page says so instead of implying the keyboard terms cover it.

## Why a second repository

A GitHub Pages site can have only one custom domain. `app-codeshift` already uses `codeshift.mygig.life`, and that name is the registered privacy URL. Moving it to `css.mygig.life` would have broken the Play listing links unless the old name was redirected, and the old name could not be served by GitHub once its custom domain changed. So the site lives in its own small public repository, `myGIGlife-claude/codeshift-site`, and the legal repository is untouched. If the legal pages are ever moved under `css.mygig.life`, do it with Cloudflare redirect rules that keep the original paths, then update both Play listings.

## Hosting and DNS

- GitHub Pages, branch `main`, folder `/`, custom domain `css.mygig.life` (the `CNAME` file contains exactly that).
- Cloudflare DNS: `css` CNAME to `mygiglife-claude.github.io`, DNS only (not proxied), TTL auto. Nothing else was changed.
- HTTPS is enforced by GitHub Pages.

## Accessibility and performance

- Semantic landmarks, one `h1` per page, a skip link, visible focus on every control, 48px minimum touch targets, real alt text, forced-colors support.
- Automated WCAG 2.0 A/AA, 2.1 AA and 2.2 AA scan (axe-core) at 390 and 1440 on all pages: no violations. Automated scans do not replace a manual screen-reader pass.
- No horizontal scrolling at 344 (foldable cover screen), 360, 390, 430, 640, 690 (unfolded foldable), 768, 820, 884, 1024 and 1440. The layout has steps at 560, 640 and 880 px so mid-size screens such as an unfolded Galaxy Z Fold use two columns instead of a stretched phone layout.
- Images are WebP at 640px wide with width and height set, so nothing shifts. The only render-blocking assets are one stylesheet and two preloaded fonts.

## Growing the site

- Add `/keyboard/` and `/terminal/` pages when each app has enough to say; move the matching section out of the home page and keep a short teaser there. The stylesheet already has no page-specific selectors.
- When an app is published, replace its "closed testing" line with a Google Play link.
- Add sitemap entries and pages in `sitemap.xml`.
- Screenshots come from `docs/store/src/shot3-*.png` and `term-shot-*.png` in the app repository. Re-export them with Pillow at 640px wide.
