# Skills

Personal agent skills for any agent that supports the skills protocol.

## Installing Skills

```bash
# install all skills
npx skills add scross01/skills

# install a single skill
npx skills add scross01/skills@<skill-name> 
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

