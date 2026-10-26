---
name: semble
description: Use when you need to find code by meaning, especially when exact names are unknown, or trace how behavior works across files.
---

# Semble

Semble finds code by meaning rather than keywords. Use it for questions such as "How does X work?", "Where is X implemented?", and cross-cutting concerns. It returns relevant code chunks with file:line locations and relevance scores.

Use ripgrep (`rg`) for known symbols and exhaustive reference searches. Semantic search ranks likely matches and may omit occurrences. Read promising results for context.

Run searches from the repository root. Describe the behavior or concept you want to find:

```bash
semble search "retry requests with exponential backoff"
```

The current directory is the default search scope. Pass local paths or Git URLs to search other repositories. Search code by default; use `--content docs`, `--content config`, or `--content all` to include other content:

```bash
semble search "database connection pool" ./src
semble search "deployment guide" --content docs
semble search "database host and port" --content config
semble search "authentication flow" ./service-a ../service-b --content all
```

Use `--top-k N` or `-k N` to control the result count. `--max-snippet-lines N` limits snippet length; `--max-snippet-lines 0` returns locations without code. Add `--format text` for readable output instead of the default JSON.

Use `semble find-related FILE LINE` to find code similar to a promising result. It accepts the same paths and result options as `search`:

```bash
semble find-related src/auth.py 42
```

Indexes build and update automatically. If `semble` is unavailable, use `uvx semble` with the same arguments.

If a search misses, rephrase the query, broaden the paths or content selection, or increase `--top-k`. If neither `semble` nor `uvx` is available, explain that semantic code search is unavailable.
