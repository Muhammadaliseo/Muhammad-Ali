# Muhammad Ali — SEO &amp; AEO Specialist Portfolio

Live site: **https://muhammadaliseo.github.io/Muhammad-Ali/**

A single-file static portfolio. No build step, no dependencies — open `index.html`
in a browser and it runs.

## Layout

```
index.html                  the whole site (markup, CSS and JS inline)
favicon.png                 32x32 tab icon
apple-touch-icon.png        180x180 iOS home-screen icon
assets/
  og-cover.png              1200x630 social share card
  projects/<slug>.png       project card screenshots
  gsc/<name>.png            Google Search Console result screenshots
  resume/*.pdf              the downloadable resume
  resume/resume-data.js     same PDF, base64 — see below
```

## Two things worth knowing before editing

**Project screenshot filenames are derived, not arbitrary.** Each card takes its
image from the project's own hostname, minus `www.` and the TLD — so
`nebulawellness.ca` looks for `assets/projects/nebulawellness.png`. Rename the
file and the card silently falls back to an auto-generated website snapshot, then
to an emoji tile. Add a project, add a matching screenshot.

**The resume exists twice and both copies must be updated together.**
`assets/resume/resume-data.js` holds the same PDF base64-encoded. It's what makes
the download work when `index.html` is opened straight off disk, because Chrome
blocks `fetch()` on `file://` URLs. Swap the PDF without regenerating the JS and
the offline download quietly keeps serving the old document:

```powershell
$pdf = "assets/resume/Muhammad-Ali-SEO-Specialist-Resume.pdf"
$b64 = [Convert]::ToBase64String([IO.File]::ReadAllBytes($pdf))
Set-Content "assets/resume/resume-data.js" "window.RESUME_B64 = `"$b64`";" -Encoding utf8
```

## Images load lazily

Thumbnails carry their URL in `data-src` and are only fetched once the card comes
within 350px of the viewport, fading in over a shimmer placeholder. If you add
images, follow the same pattern or they'll load eagerly and slow the first paint.
