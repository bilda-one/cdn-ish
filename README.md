# CDN-ish

A personal store of static assets consumed **remotely by URL** — no build step, no package manager, nothing to install. Files are served straight from GitHub:

```text
https://raw.githubusercontent.com/bilda-one/cdn-ish/main/<path>
```

Point a host application's setting at the URL of the asset you want and it fetches its own copy; every machine configured that way then follows `main` with no further work.

## Assets

| Folder                                           | Asset                                                                                            | Consumed by                         |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------ | ----------------------------------- |
| [`css/markdown-preview/`](css/markdown-preview/) | `markdown-style.css` — an override layer on top of VS Code's built-in markdown preview styling   | VS Code's `markdown.styles` setting |
| [`schema/`](schema/)                             | JSON Schemas, one flat folder, one file per subject                                              | a document's `$schema`              |

Each folder carries its own `README.md` giving the exact URL to paste and the setting that takes it — start there rather than from the asset file.

## What to expect from these URLs

- **`main` is live and unversioned.** A push is fetchable immediately, and there is nothing to pin to: a consumer always gets the current file.
- **Paths are the contract.** They get added, never moved or renamed, because a rename silently breaks every machine already pointing at the old URL.
- **A stale copy can linger.** raw.githubusercontent serves with a short cache TTL, so a consumer that fetched the previous version may keep serving it for a few minutes after a push.
- **Everything arrives as `text/plain`.** That is what the consuming tools want — an editor resolving a `$schema`, VS Code loading a stylesheet — but a browser shows CSS or HTML as text instead of rendering it.

## How it is published

The public repo is a derived artifact. It is generated from a private working repo that holds the same assets plus everything that stays unpublished — reference docs, test fixtures and the release script — with an allow-list deciding which paths reach the public side, at identical paths. Each release lands as a single commit on top of the last, so a change pushed straight to the public repo would be overwritten by the next one.
