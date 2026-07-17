# website

The `valiss.dev` apex site: a Hugo build deployed to GitHub Pages by the `site`
workflow. It renders the landing page, the user documentation (`/docs/`), the
specification (`/spec/`), and the Go vanity import metadata that lets
`go get valiss.dev/...` resolve.

DNS and the Pages resource (`build_type = "workflow"`) are managed in the
private `infra` repo.

## Layout

- `hugo.toml` — site config: the module imports (theme and content), content
  mounts, menus, and Hextra params.
- `go.mod` / `go.sum` — this site is a Hugo Module; the theme and the content
  repos are its module requirements, version-pinned here.
- `data/vanity.yaml` — the registry of Go module roots (ADR 0016), the single
  source of truth for the `valiss.dev` Go namespace. One entry per module root.
- `content/_content.gotmpl` — generates one vanity page per registry entry at
  the module's path (`valiss.dev/valiss` -> `/valiss/`).
- `content/_index.md` — the landing page (Hextra home layout).
- `layouts/vanity.html` — the standalone vanity page template (see the
  override inventory below).
- `static/` — `CNAME` and `.nojekyll`, copied verbatim into the output.
- `.github/workflows/site.yaml` — builds with the pinned plain (non-extended)
  Hugo, greps the rendered `go-import`/`go-source` metas for every registry
  entry, and deploys via the Pages workflow path.

Build locally: `hugo` (plain binary suffices; the site must not grow a
dependency on extended-only features). Output in `public/`.

## Theme (ADR 0018)

The [Hextra](https://github.com/imfing/hextra) theme is imported as a Hugo
Module for the whole site (landing, docs, spec). It ships precompiled CSS, so
the plain Hugo binary builds it with no Node and no extended features.

Customization discipline, in escalating order (ADR 0018): `hugo.toml` config
first; then CSS via Hextra's `custom.css` hook and hue variables; then shadowed
templates only when config and CSS cannot express the need. Every project-level
template is inventoried below; the inventory is the theme-upgrade friction
budget. Upgrading Hextra is a one-line version bump in `go.mod`
(`hugo mod get github.com/imfing/hextra@vX.Y.Z`) reviewed against that inventory.

### Override inventory

- `layouts/vanity.html` — not a Hextra shadow. A standalone, chrome-free page
  (`go-import`/`go-source` metas plus a pkg.go.dev redirect) selected by the
  registry-generated vanity pages via their `layout: vanity`. Kept minimal and
  theme-independent on purpose: its byte output feeds `go get` resolution and
  the workflow's vanity-meta grep, so it must not inherit theme markup.

## Content mounts (ADR 0011)

Content lives in separate pure-Markdown repos and is mounted into the section
tree as Hugo Modules:

- `github.com/valiss-dev/docs` — its `content/` directory mounts to
  `content/docs`, rendering under `/docs/`. (The repo's `adr/` records are not
  mounted and do not render on the site.)
- `github.com/valiss-dev/spec` — its root `SPEC-*.md` mounts to `content/spec`,
  rendering under `/spec/`. The conformance `vectors/` and repo metadata are
  excluded from the mount (`files = "/SPEC-*.md"`).

### Pinning and bumping content

Each content repo is pinned to an exact revision in `go.mod`, so a site rebuild
renders a deliberate content revision, not whatever is on a branch (ADR 0011).
To publish newer content, bump the pin and commit `go.mod`/`go.sum`:

```
hugo mod get github.com/valiss-dev/docs@latest   # or @<commit>/@<tag>
hugo mod get github.com/valiss-dev/spec@latest
```

The mount is defined against a fixed target path, so it stays valid even when
the pinned content is sparse (a section renders from whatever pages exist).

## Pages hosting

Pages builds from the `site` workflow artifact (`build_type = "workflow"`,
set in `infra`), not from a branch. The vanity page for `valiss.dev/valiss`
serves a live, published module: the Hugo build renders it byte-identical to
its historical static form, and the workflow greps every registry entry's
`go-import`/`go-source` metas to keep `go get` resolution from silently
regressing.
