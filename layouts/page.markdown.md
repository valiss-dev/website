{{- /* Raw-Markdown alternative of a content page. Mirrors the theme default but
       honors hideTitle: pages whose mounted source already opens with its own H1
       (the spec documents) must not get a second title prepended. */ -}}
{{- if not .Params.hideTitle -}}
{{ .Title | replaceRE "\n" " " | printf "# %s" }}

{{ end -}}
{{- .RawContent -}}
