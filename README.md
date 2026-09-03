# goappsters.in

Single-page static site for GoAppsters Private Limited. No framework, no build step, no JavaScript, no analytics.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site, seven sections in order: Hero, Proof (three cards plus a "More work" list), Offers, How I work, Capabilities, About, Contact |
| `styles.css` | All styling. System font stack, one orange accent, light and dark via `prefers-color-scheme` |
| `favicon.svg` | Typographic "G" favicon |
| `og.png` | 1200×630 link-preview image, generated from the headline text |
| `images/` | Kaal Jyoti and Samudrik screenshots pulled from the Play Store listings, 400 px wide WebP |
| `sitemap.xml`, `robots.txt` | Single-URL sitemap and a permissive robots file |
| `CNAME` | Custom domain for GitHub Pages |

## Deploy

The repo is served by GitHub Pages from the `main` branch with the custom domain in `CNAME`.

```bash
git add -A && git commit -m "Rebuild site" && git push origin main
```

GitHub Pages usually updates within a minute or two. To host anywhere else, copy every file in the repo root (except `.git` and `README.md`) to the web root of any static host. Nothing needs compiling.

## Placeholder checklist

None. Every item on the page is final.

## Image recommendations

Keep the page under about 400 KB total so it stays fast on mobile data.

| Image | Size | Format | Notes |
|---|---|---|---|
| Kaal Jyoti screenshots (×3) | 390×844 px (or the phone's native ratio), display width is about 110 px on phones and 150 px on desktop, so 400 px wide is plenty | WebP, quality 75–80, target under 60 KB each | Set `width` and `height` attributes, `loading="lazy"`, `decoding="async"`, and a descriptive `alt` |
| `og.png` | 1200×630 px | PNG or JPG, under 300 KB | Not lazy, referenced from `<head>` only. Plain background, wordmark, and the headline works well |

Put images in an `images/` folder. To convert on a Mac: `sips -s format webp -Z 400 in.png --out images/out.webp` (or use Squoosh in the browser).

## Lighthouse results (measured 2026-09-03)

Lighthouse 12, headless Chrome, served from a local `python3 -m http.server` before any real images were added.

| Preset | Performance | Accessibility | Best practices | SEO |
|---|---|---|---|---|
| Mobile (default, throttled) | 100 | 100 | 100 | 100 |
| Desktop | 100 | 100 | 100 | 100 |

Mobile metrics: FCP 0.75 s, LCP 0.90 s, CLS 0, TBT 0 ms. Desktop: FCP 0.20 s, LCP 0.24 s, CLS 0.

Re-measure after adding images. With the sizes above, performance should stay at 95+. If LCP climbs, the likely cause is an oversized screenshot or headshot.

To re-run locally:

```bash
python3 -m http.server 8766 & npx lighthouse@12 http://127.0.0.1:8766/ --view --chrome-flags="--headless=new"
```

## Accessibility notes

Heading order is one `h1`, one `h2` per section, `h3` for cards. Every colour pair used for text was checked against WCAG AA (lowest ratio 4.85:1, orange accent on the light background). Focus states are a 3 px accent outline. Placeholder image boxes carry `role="img"` and an `aria-label` so screen readers don't hit empty regions.

## Analytics (optional, not installed)

None is included and none is needed for the site's purpose, since visitors arrive from your own outreach. If you later want to know whether people click WhatsApp or the booking link, a privacy-respecting option is Plausible (cookie-free, no consent banner needed) or a self-hosted Umami. Either is a single `<script>` tag before `</body>`, and you can track the two buttons with a `data-` attribute or a class. Adding one script will cost a few Lighthouse performance points on mobile.
