---
name: searxngr
description: >
  Web search via SearXNG using the searxngr CLI. Use instead of webfetch
  when you need to search the web, look up current info, verify facts, or
  research topics. Supports targeted searches via specific engines and
  categories with JSON output for easy piping.
---

# SearXNG Search (searxngr)

A privacy-respecting metasearch engine CLI for agents. Use this instead of
webfetch or web_search when you need real search results from multiple sources.

## Setup

```bash
uv tool install https://github.com/scross01/searxngr.git
```

All configuration is via CLI flags. Set your SearXNG instance with `--searxng-url`:

```bash
searxngr --searxng-url https://search.example.com "query"
```

## Core Usage

```bash
# Basic search — returns top 5 results with full URLs
searxngr --searxng-url https://search.example.com -x "rust async runtime comparison"

# More results, pipe-friendly JSON output
searxngr --searxng-url https://search.example.com --json -n 10 "kubernetes pod scheduling"

# Search within a domain
searxngr --searxng-url https://search.example.com -w docs.rs "tokio spawn"
```

## Discovering Engines and Categories

Before targeting specific engines, discover what your instance supports:

```bash
# List all engines on your instance
searxngr --searxng-url https://search.example.com --list-engines

# List all categories and their engines
searxngr --searxng-url https://search.example.com --list-categories
```

Use the engine names from `--list-engines` output with `-e` for targeted searches.

## Targeted Search with Engines

The real power: skip generic search and hit specific engines directly.

```bash
# Code and packages — search GitHub, npm, crates.io
searxngr --searxng-url $URL -e github "searxng python"
searxngr --searxng-url $URL -e npm "react table component"
searxngr --searxng-url $URL -e crates.io "async file io"

# Documentation — hit MDN, Arch Wiki, Stack Overflow
searxngr --searxng-url $URL -e mdn "fetch API timeout"
searxngr --searxng-url $URL -e arch_linux_wiki "systemd service"
searxngr --searxng-url $URL -e stackoverflow "docker compose volume permission"

# Academic and research
searxngr --searxng-url $URL -e arxiv "transformer attention mechanisms"
searxngr --searxng-url $URL -e google_scholar "rust vs go performance"

# News — recent coverage from specific outlets
searxngr --searxng-url $URL -e google_news -t week "linux kernel release"
searxngr --searxng-url $URL -e reuters -t day "AI regulation"

# Package registries
searxngr --searxng-url $URL -e docker_hub "nginx alpine"
searxngr --searxng-url $URL -e npm -w npmjs.com "eslint config"
```

## Categories

Categories group engines by domain. Use `-c` to scope:

| Category | Use for |
|----------|---------|
| `general` | Broad web search (default) |
| `news` | Current events, recent articles |
| `it` | Programming, dev tools, package registries |
| `science` | Academic papers, research |
| `files` | Torrents, file sharing |
| `videos` | Video content |
| `images` | Image search |
| `music` | Audio content |
| `social+media` | Social platforms |

```bash
searxngr --searxng-url $URL -c it "best practices for postgresql indexing"
searxngr --searxng-url $URL -c news -t day "rust 1.80 release"
searxngr --searxng-url $URL -c science -t month "CRISPR gene editing"
```

Combine categories: `-c it -c science`.

## Time Filtering

```bash
searxngr --searxng-url $URL -t week "latest elasticsearch release"    # past week
searxngr --searxng-url $URL -t month "vue 3 migration guide"           # past month
searxngr --searxng-url $URL -t year "terraform aws best practices"     # past year
```

## Piping and Programmatic Use

Always use `--json` when piping results to other commands or processing programmatically:

```bash
# Extract title, snippet, and URL from results
searxngr --searxng-url $URL --json -n 10 "rust async" | jq '.results[] | {title, snippet, url}'
```

## Quick Reference

| Flag | Purpose |
|------|---------|
| `-n N` | Results per page (default 5, 0 for server default) |
| `-e ENGINE` | Target specific engine(s) |
| `-c CAT` | Category filter (general, news, it, science, ...) |
| `-t RANGE` | Time range: day, week, month, year |
| `-w SITE` | Restrict to domain |
| `-l LANG` | Language (en, de, fr, ...) |
| `--json` | JSON output for piping |
| `--searxng-url URL` | Override instance URL |
| `--no-verify-ssl` | Skip SSL verification (self-signed certs) |

## When to Use This

- **Search for current info** — not in your training data or you need freshness
- **Research a topic** — get multiple perspectives from different sources
- **Find code/examples** — target GitHub, npm, crates.io, Stack Overflow directly
- **Verify claims** — cross-reference across engines
- **Alternative to web_search** — when you need more control over engines and output format
