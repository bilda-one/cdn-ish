# CDN-ish

A personal store of static assets consumed **remotely by URL** — no build step, no package manager, nothing to install. Files are served straight from GitHub:

```text
https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/<path>
```

The `refs/heads/main` form is the one to paste. `.../cdn-ish/main/<path>` serves the same bytes, but a schema that pins its own `$schema` with a `const` accepts only the exact URL above and reports the short form as invalid.

## CSS

| File | Description | URL |
| --- | --- | --- |
| [`markdown-style.css`](css/markdown-preview/markdown-style.css) | An override layer on top of VS Code's built-in markdown preview styling — point the `markdown.styles` setting at it | [url](https://cdn.jsdelivr.net/gh/bilda-one/cdn-ish@main/css/markdown-preview/markdown-style.css)[^1] |

[^1]: **Why not a raw URL:** `raw.githubusercontent.com` serves every file as `text/plain` with `nosniff`, and a `<link rel="stylesheet">` only applies a response typed `text/css` — a raw URL loads the bytes and the preview renders unstyled, with no error. jsDelivr serves the same file from GitHub with the right type. The trade-off is freshness: `@main` is cached for hours there, against minutes on raw.

## JSON Schema

### Schema files

Point a document's `$schema` at the URL and any editor validates and completes it, with no local setup.

| File | Description | URL |
| --- | --- | --- |
| [`restic-backup.schema.json`](schema/restic-backup.schema.json) | Backup definitions for restic batch script | [url](https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/schema/restic-backup.schema.json) |

### Definition files

Building blocks, not consumables: neither file validates anything on its own, and no document points its `$schema` at one. The schemas can reach them with `$ref`.

| File | Description | URL |
| --- | --- | --- |
| [`types.schema.json`](schema/definitions/types.schema.json) | Value types (i.e. string shapes and their patterns) | [url](https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/schema/definitions/types.schema.json) |
| [`enums.schema.json`](schema/definitions/enums.schema.json) | Value enums (closed lists of allowed values) | [url](https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/schema/definitions/enums.schema.json) |
