---
name: tabletop
description: >
  Reformat, sort, filter, and reshape space-aligned CLI table output. Use
  when you need to parse tabular output from commands like df, docker ps,
  ollama list, or ps and convert it to JSON, CSV, Markdown, or pipe it
  through transforms.
---

# Tabletop (tabletop)

Parse and transform space-aligned CLI tables. Many tools output tables with
columns aligned using spaces — hard to sort, filter, or pipe. tabletop fixes that.

## Setup

```bash
uv tool install https://github.com/scross01/tabletop.git
```

## Core Usage

```bash
# Pipe any space-aligned table
df -h | tabletop

# Sort by column (smart unit detection for sizes, times)
df -h | tabletop -sr Used

# Export to JSON, CSV, or Markdown
docker ps | tabletop --json
docker ps | tabletop --csv
docker ps | tabletop --markdown
```

## Transforms

```bash
# Filter rows by regex
ollama list | tabletop -f name:gemma

# Select specific columns (by name or index)
ps -eaf | tabletop -c PID,CMD
docker ps | tabletop -c 1,2,3

# Remove columns
df -h | tabletop -r Use%

# Head/Tail
ollama list | tabletop -H 5
ollama list | tabletop -T 3

# Deduplicate
docker ps | tabletop -u image

# Combine transforms
ollama list | tabletop -s 3 -f name:gemma -c name,size
```

## Quick Reference

| Flag | Purpose |
|------|---------|
| `-s COL` | Sort ascending by column |
| `-sr COL` | Sort descending by column |
| `-f COL:PAT` | Filter rows where column matches regex |
| `-c COLS` | Show only these columns |
| `-r COLS` | Remove these columns |
| `-H N` | Keep first N rows |
| `-T N` | Keep last N rows |
| `-u [COL]` | Deduplicate rows |
| `--json` | JSON array of objects |
| `--csv` | CSV output |
| `--markdown` | Markdown table |
| `--stats` | Column statistics |
| `--no-header` | Treat first row as data |

## When to Use This

- **Parse CLI output** — `df`, `docker ps`, `ps`, `ollama list`, `netstat`
- **Filter/sort results** — extract specific rows or order by column
- **Export to structured formats** — JSON, CSV, Markdown for piping or reporting
- **Column selection** — show only what matters, hide the rest
