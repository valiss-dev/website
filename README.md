# website

The `valiss.dev` apex site, served by GitHub Pages. Its only load-bearing job is
hosting the Go vanity import metadata; the landing page is incidental.

- `valiss/index.html` — carries the `go-import` meta tag for `valiss.dev/valiss`.
  A `go get valiss.dev/valiss/<subpkg>` request that 404s on the subpath walks up
  to `/valiss`, so this one page covers every subpackage (ADR 0003).
- `index.html` — human landing page.
- `CNAME` — custom domain (`valiss.dev`); also set via the Pages config in the
  `infra` repo.
- `.nojekyll` — serve files verbatim, no Jekyll processing.

DNS and the Pages repo settings are managed in the private `infra` repo.
