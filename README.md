# CDN-ish

A personal store of static assets consumed **remotely by URL** — no build step, no package manager, nothing to install. Files are served straight from GitHub:

```text
https://raw.githubusercontent.com/bilda-one/cdn-ish/main/<path>
```

## Assets

| File | Type | Description | URL |
| --- | --- | --- | --- |
| [`markdown-style.css`](css/markdown-preview/markdown-style.css) | CSS | An override layer on top of VS Code's built-in markdown preview styling | [url](https://cdn.jsdelivr.net/gh/bilda-one/cdn-ish@main/css/markdown-preview/markdown-style.css)[^1] |
| [`restic-backup.schema.json`](https://github.com/bilda-one/cdn-ish/blob/main/schema/restic-backup.schema.json) | JSON Schema | Backup definitions for restic batch script | [url](https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/schema/restic-backup.schema.json) |
| [`types.schema.json`](https://github.com/bilda-one/cdn-ish/blob/main/schema/definitions/types.schema.json) | JSON schema definition[^2] | Value types (i.e. string shapes and their patterns) | [url](https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/schema/definitions/types.schema.json) |
| [`enums.schema.json`](https://github.com/bilda-one/cdn-ish/blob/main/schema/definitions/enums.schema.json) | JSON schema definition[^2] | Value enums (closed lists of allowed values) | [url](https://raw.githubusercontent.com/bilda-one/cdn-ish/refs/heads/main/schema/definitions/enums.schema.json) |

[^1]: **Why not a raw URL:** `raw.githubusercontent.com` serves every file as `text/plain` with `nosniff`, and a `<link rel="stylesheet">` only applies a response typed `text/css` — a raw URL loads the bytes and the preview renders unstyled, with no error. jsDelivr serves the same file from GitHub with the right type. The trade-off is freshness: `@main` is cached for hours there, against minutes on raw.

[^2]: Building blocks, not consumables: neither file validates anything on its own, and no document points its `$schema` at one. The schemas can reach them with `$ref`.
