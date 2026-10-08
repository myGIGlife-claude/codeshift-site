# CodeShift website

Source of https://css.mygig.life (GitHub Pages, branch `main`, folder `/`). Plain HTML and CSS, no build step, no JavaScript, no third-party requests.

- `index.html` home page, `privacy/index.html` guide to the privacy documents, `404.html`
- `styles.css` the only stylesheet, `fonts/` self-hosted woff2 (SIL OFL), `img/` WebP screenshots and icons
- `CNAME` must contain exactly `css.mygig.life`
- Design and architecture notes: `DESIGN_DECISIONS.md`

The privacy policies and terms live in `myGIGlife-claude/app-codeshift` at `codeshift.mygig.life` and must not be moved: that address is registered in the Google Play listings.

Preview locally: `python3 -m http.server 8000` in this folder.
