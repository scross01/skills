# Skills

Personal agent skills for any agent that supports the skills protocol.

## Installing Skills

```bash
npx skills add scross01/skills@<skill-name> -g -y
```

### Prerequisites

Some skills require additional tools. Install them with `uv`:

```bash
# Required for searxngr skill
uv tool install https://github.com/scross01/searxngr.git

# Required for fetch skill
uv tool install https://github.com/scross01/fetch.git

# Required for tabletop skill
uv tool install https://github.com/scross01/tabletop.git

# Required for keeenv skill
uv tool install https://github.com/scross01/keeenv.git
```

## Available Skills

| Skill | Description |
|-------|-------------|
| [fetch](fetch/SKILL.md) | Fetch web pages and convert to clean text — GitHub, YouTube, RSS, OG metadata |
| [keeenv](keeenv/SKILL.md) | Securely load secrets from KeePass — dotenv replacement for API keys and passwords |
| [searxngr](searxngr/SKILL.md) | Web search via SearXNG — targeted engine searches, JSON piping, agent-friendly |
| [tabletop](tabletop/SKILL.md) | Parse and transform CLI table output — sort, filter, export to JSON/CSV/Markdown |

## Creating New Skills

Each skill lives in its own directory with a `SKILL.md` file:

```
skills/
├── README.md
├── my-skill/
│   └── SKILL.md
```

The `SKILL.md` frontmatter requires at minimum:

```yaml
---
name: my-skill
description: What this skill does
---
```
