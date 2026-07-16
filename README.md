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

## Hugo build (pending cutover)

The Hugo sources coexist with the static files above and are inert while Pages
builds from the branch root (`build_type = "legacy"`):

- `hugo.toml` — minimal theme-less config.
- `data/vanity.yaml` — the registry of Go module roots (ADR 0016), the single
  source of truth for the `valiss.dev` Go namespace. One entry per module
  root; `content/_content.gotmpl` generates a vanity page per entry at the
  module's path (`valiss.dev/valiss` → `/valiss/`), rendered by
  `layouts/vanity.html`. Adding a module root is one registry line, added
  together with its repository.
- `layouts/home.html` — the landing page.
- `static/` — `CNAME` and `.nojekyll`, copied verbatim into the output.
- `.github/workflows/site.yaml` — builds with pinned non-extended Hugo,
  greps the rendered `go-import`/`go-source` metas for every registry entry,
  and deploys via the Pages workflow path.

Build locally: `hugo` (plain binary suffices; the site must not grow a
dependency on extended-only features), output in `public/`.

## Cutover runbook: legacy branch build → Hugo workflow build

The vanity page for `valiss.dev/valiss` serves a live, published module and
must not break at any step. The Hugo build renders it byte-identical to the
static file, so both build types serve the same metas.

1. Push `main` (Hugo scaffold + `.github/workflows/site.yaml`). Pages still
   serves the branch root; the workflow's deploy job fails until step 2, which
   is expected.
2. In `infra`, apply the prepared Pages flip
   (`github_repository_pages.website` `build_type` `"legacy"` → `"workflow"`):
   `make tofu/apply`.
3. Re-run the `site` workflow on `main` (its pre-flip run could not deploy)
   and wait for it to go green, then verify:
   - `curl -s 'https://valiss.dev/valiss?go-get=1' | grep go-import`
   - `go list -m valiss.dev/valiss@latest` (use `GOPROXY=direct` to bypass
     the module proxy cache)
4. Follow-up commit: remove the legacy root static files, now superseded by
   the Hugo build — `index.html`, `valiss/index.html`, `CNAME`, `.nojekyll`
   (their Hugo sources live in `layouts/` and `static/`).

Rollback: flip `build_type` back to `"legacy"` (restoring the `source` block)
in `infra` and apply; Pages serves the branch root again. This only works
before step 4 removes the root static files, so do not perform step 4 until
the cutover is verified.
