{{- /* Raw-Markdown alternative of a section index. Mirrors the theme default but
       honors hideTitle, matching layouts/docs/single.html's HTML behavior. */ -}}
{{- if not .Params.hideTitle -}}
{{ .Title | replaceRE "\n" " " | printf "# %s" }}

{{ end -}}
{{- .RawContent -}}
