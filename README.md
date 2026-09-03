# goappsters.in

Single-page static site for GoAppsters Private Limited. No framework, no build step. The only JavaScript is the Google Analytics tag and a four-line click tracker.

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

## Analytics

Google Analytics 4 is installed (measurement ID `G-W0GXQTQZMV`) via the standard gtag snippet in `<head>`. A small inline script at the end of `<body>` sends a `cta_click` event with a `cta` parameter (`whatsapp-hero`, `whatsapp-contact`, `booking-hero`, `booking-contact`) when a visitor taps one of the four buttons. In GA4, mark `cta_click` as a key event under Admin, then Events, to see conversions. GA4 sets cookies, so add a consent notice if you start getting meaningful EU traffic.
