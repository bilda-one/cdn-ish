# JSON Schemas

One flat folder, one schema per file, named `<subject>.schema.json`. Point a document's `$schema` at the raw URL and any editor validates and completes it:

```text
https://raw.githubusercontent.com/bilda-one/cdn-ish/main/schema/<file>
```

| File                                          | Describes                                                                       |
| --------------------------------------------- | ------------------------------------------------------------------------------- |
| [`restic-backup.schema.json`](#restic-backup) | Backup definitions for the standalone `Invoke-Backup.ps1` restic script         |
| `_definition.schema.json`                     | Value types shared by the schemas above — reached by `$ref`, never by `$schema` |

`_definition.schema.json` is a building block, not a consumable: it validates nothing on its own. The sibling schemas reach it with a relative `$ref` (`_definition.schema.json#/definitions/resticTag`), which the editor resolves against the schema's own URL — so both files have to sit in the same folder, and a new shared type is added there rather than inline. Names are plain when the type is generic (`nonBlankString`) and tool-prefixed when its rule comes from one tool (`resticTag`).

## restic-backup

```json
{
    "$schema": "https://raw.githubusercontent.com/bilda-one/cdn-ish/main/schema/restic-backup.schema.json",
    "repository": "%BackupPath%\\restic",
    "tags": ["app", "config"],
    "backups": [
        {
            "name": "KeePassXC",
            "paths": ["%APPDATA%\\KeePassXC"]
        },
        {
            "name": "Flow Launcher",
            "paths": ["%APPDATA%\\FlowLauncher"],
            "exclude": ["*.bak", "Cache", "Logs"]
        }
    ]
}
```

One `backups` entry is one restic snapshot holding all of its `paths`, tagged with the entry `name` plus the global and entry-level `tags`. Paths use `%VAR%` environment variable syntax and are expanded repeatedly, so a variable pointing at another variable resolves. There is no `include`: to back up a single file out of a large folder, put the file's own path in `paths`.
