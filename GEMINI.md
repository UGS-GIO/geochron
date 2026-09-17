# geochron — PR review guide
Static ArcGIS JS 4.24 + calcite-maps/Bootstrap 3 map app (public/index.html) that plots UGS
geochronology data from WFS/FeatureLayers. Older maintenance app — not being modernized. Review
ONLY the changed lines (general bug/security/quality assumed). Cite file:line; group nits; this app
is in maintenance — prefer minimal, in-style fixes over refactors.

## Match the existing code
- jQuery / Dojo-style `require([...])` / ArcGIS-4 / Bootstrap-3 era. Match surrounding patterns;
  don't add frameworks, a build step, or rewrite working popup/table code.
- Pinned CDN versions (ArcGIS 4.24, Bootstrap 3.3.5, calcite-maps 0.10) — don't bump casually.

## Security (the priority for a public legacy app)
- NO secrets, API keys, or ArcGIS tokens committed in client-side JS/HTML — flag any hardcoded
  credential or `token=`. (A Firebase web `apiKey`, if present, is designed to be public — don't
  false-flag it; the real risk is service/DB creds or privileged tokens.)
- XSS / DOM injection: popups and the data grid are built by writing feature attributes into
  `innerHTML` (index.html ~1524+). Any new field rendered from a WFS/Feature response or a URL query
  param must be escaped — no unescaped `innerHTML`/`document.write` of user/feature/URL data.

## Correctness
- Fail loud on WFS/query errors — don't silently blank the map or grid; handle empty/failed responses.
