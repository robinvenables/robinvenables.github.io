---
title: 'Creating the Old Website Archive'
date: '2025-05-03T07:00:00+03:00'
draft: false
categories: ["website"]
tags: ["hugo"]
summary: 'Having decided to include some of the old website as an Archive section, I needed a way to present this. I chose to implement it as a menu item, linking to a series, a feature provided by the Anatole theme.'
---

Having decided to include some of the old website as an Archive section, I needed a way to present this. I chose to implement it as a menu item, linking to a series, a feature provided by the Anatole theme.

The first step was to create the menu in the Hugo configuration file (menu.toml):

```toml
[[main]]
    name = 'Archive'
    identifier = 'archive'
    weight = 200
    url = '/series/archive'
```

Then, I added the series parameter to the front matter for each page:

```yaml
series: ["archive"]
```

By default, the theme adds a list of all the posts in the series to the end of the page. As there is a archive list already, this seemed an unnecessary duplication. I needed to check:

- If the series parameter exists in front matter.
- For a non-empty series array
- That the first item is not "archive"; these posts will only ever be part of the archive series so it will always be the first item in the array.

I copied single.html from the theme into layouts/_default and updated it to check for all of these conditions before calling the series partial:

```go
{{ if and (isset .Params "series") (gt (len .Params.series) 0) (ne (index .Params.series 0) "archive") }}
    {{- partial "series.html" . -}}
{{- end -}}
```

You can see the result by clicking the ARCHIVE menu above.