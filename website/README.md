# Website

This folder holds the release website for Hermes BIOS-mode.

## Files

- `index.html` — a single self-contained landing page (inline CSS, no build step, no external dependencies). Warm, light, homey palette (cream `#fff8f1` + orange `#fa5d00`).

## Deploy

The page is static and dependency-free. Any static host works:

- **GitHub Pages** — push the repo, enable Pages, point it at the `website/` folder (or the repo root if you move `index.html` there).
- **Netlify / Vercel / Cloudflare Pages** — set the publish directory to `website/`.
- **Any web server** — copy `index.html` to the docroot.

## Notes

- The FAQ, changelog, and hardware map are linked from the footer as relative paths. If you deploy `index.html` standalone (without the rest of the repo), those links will 404 — either deploy the whole repo or adjust the links to absolute URLs.
- To change the palette, edit the CSS custom properties in the `:root` block at the top of `index.html`.
