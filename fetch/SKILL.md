---
name: fetch
description: >
  Fetch web pages and convert to clean, readable text. Use when you need
  to read a specific URL, extract content from articles, GitHub repos,
  YouTube transcripts, or RSS feeds. Preferred over webfetch for full-page
  content retrieval.
---

# Fetch (fetch)

A lightweight, zero-config web fetch CLI for agents. Use this instead of
`webfetch` when you need clean, structured content from a specific URL.

## Setup

```bash
uv tool install https://github.com/scross01/fetch.git
```

## Core Usage

```bash
# Fetch a page as Markdown (default)
fetch https://example.com

# Plain text output
fetch --format txt https://example.com

# JSON output with metadata
fetch --format json https://example.com

# Quiet mode for piping (no status messages)
fetch -q https://example.com | less

# Limit output size
fetch --max-size 5000 https://example.com
```

## GitHub URLs

No API key needed for public repos. Automatically detects and fetches:

```bash
# Repo root — fetches README
fetch https://github.com/user/repo

# Raw file via blob URL
fetch https://github.com/user/repo/blob/main/src/main.py

# Issue with comments
fetch https://github.com/user/repo/issues/42

# Pull request with review and issue comments
fetch https://github.com/user/repo/pull/42
```

## YouTube Transcripts

Extracts transcript text without an API key:

```bash
fetch https://www.youtube.com/watch?v=VIDEO_ID
fetch https://youtu.be/VIDEO_ID
```

## RSS/Atom Feeds

```bash
# Auto-detect feed from URL
fetch https://example.com/feed.xml

# Discover feed from a page's metadata
fetch --rss https://example.com/blog
```

## Open Graph Metadata

Extract `og:*` tags from any page:

```bash
fetch --og https://example.com
fetch --og --format json https://example.com
```

## Stdin Piping

Pipe URLs or raw HTML:

```bash
# Pipe a URL
echo "https://example.com" | fetch -q

# Pipe multiple URLs (results separated by ---)
printf '%s\n' "https://example.com" "https://another.com" | fetch -q

# Pipe raw HTML for conversion
curl -s https://example.com | fetch --html

# Pipe HTML file with base URL for relative links
cat page.html | fetch --html https://example.com
```

## Quick Reference

| Flag | Purpose |
|------|---------|
| `--format` | Output: `markdown` (default), `txt`, `html`, `json` |
| `-o FILE` | Write output to file |
| `-q` | Suppress status messages (use for piping) |
| `--timeout` | HTTP timeout in seconds (default 30) |
| `--max-size N` | Truncate output to N characters |
| `--html` | Treat stdin as raw HTML |
| `--og` | Extract Open Graph metadata |
| `--rss` | Discover and fetch RSS/Atom feed from page |
| `--favicon` | Extract favicon URLs |
| `--raw` | Include full response data |

## When to Use This

- **Read a specific URL** — when you have a known page to fetch
- **GitHub content** — READMEs, issues, PRs, raw files without API keys
- **YouTube transcripts** — extract video text without API keys
- **Feed consumption** — RSS/Atom parsing and discovery
- **HTML conversion** — pipe raw HTML to get clean Markdown
- **Alternative to webfetch** — when you need more control over format and extraction
