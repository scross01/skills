---
name: keeenv
description: >
  Securely populate environment variables from a KeePass database. Use
  instead of .env files when you need to load secrets, API keys, or
  credentials without storing them in plaintext.
---

# Keeenv (keeenv)

Like dotenv, but pulls secrets from a KeePass database at runtime. Never
store plaintext secrets in config files.

## Setup

```bash
uv tool install https://github.com/scross01/keeenv.git
```

## Configuration

Create a `.keeenv` file in your project root:

```ini
[keepass]
database = secrets.kdbx
keyfile = mykey.key

[env]
SECRET_USERNAME = ${"My Secret".Username}
SECRET_PASSWORD = ${"My Secret".Password}
SECRET_API_KEY = ${"My Secret"."API Key"}
DATABASE_URL = ${"Production DB"."Connection String"}
```

Standard attributes: `Username`, `Password`, `URL`, `Notes`. Custom attributes are supported — quote names with spaces.

## Subcommands

```bash
# Initialize a new .keeenv config (interactive)
keeenv init

# Initialize with explicit paths
keeenv init --kdbx ./secrets.kdbx --keyfile ./mykey.key

# Initialize with keyfile only (no master password)
keeenv init --kdbx ./secrets.kdbx --keyfile ./mykey.key --no-password

# Non-interactive init — create new kdbx without prompts
keeenv init --kdbx ./secrets.kdbx --keyfile ./mykey.key -y

# Export environment variables (eval into shell)
eval "$(keeenv eval)"

# Run a command with secrets injected
keeenv run my-command --arg1 --arg2

# List variable names (no DB connection needed)
keeenv list

# Add a new credential to KeePass and map it
keeenv add "MY_SECRET" "value"
keeenv add -t "Entry Title" -u "user" --url "https://example.com" "MY_SECRET" "value"
keeenv add --force "MY_SECRET" "new-value"

# Pipe secret from stdin
pbpaste | keeenv add "MY_SECRET"
```

## Non-Interactive Usage

Set `KEEENV_PASSWORD` to skip interactive password prompts. Useful for CI/CD,
Docker, and scripted workflows:

```bash
# Export with password from env
KEEENV_PASSWORD="my-pass" eval "$(keeenv eval)"

# Run a command non-interactively
KEEENV_PASSWORD="my-pass" keeenv run my-command

# Create new kdbx without prompts
KEEENV_PASSWORD="my-pass" keeenv init --kdbx ./secrets.kdbx -y
```

**Security note:** `KEEENV_PASSWORD` is readable by any process owned by the
same user (e.g. `/proc/<pid>/environ`). Prefer keyfile-only auth when possible:

```bash
# Keyfile-only — no password exposure
keeenv init --kdbx ./secrets.kdbx --keyfile ./mykey.key --no-password
```

## Grouped Entries

Reference entries inside KeePass groups with path syntax:

```ini
SECRET_KEY = ${"Parent/Child/Entry Title".Password}
DB_CONN = ${"Infra/Databases/Prod"."Connection String"}
```

When using `keeenv add`, groups are created automatically:

```bash
keeenv add -t "Services/Github/Token" "GITHUB_TOKEN" "ghp_xxx"
```

## Quick Reference

| Flag | Purpose |
|------|---------|
| `eval` | Print `export` commands to stdout |
| `run CMD` | Execute command with secrets injected |
| `list` | List env var names (no DB lookup) |
| `init` | Create `.keeenv` config |
| `add VAR [SECRET]` | Add credential to KeePass + map in config |
| `--config PATH` | Custom config path (default: `.keeenv`) |
| `--strict` | Fail if any placeholder can't be resolved |
| `--quiet` | Errors only |
| `--verbose` | Debug output |
| `-y` | Auto-confirm init (create new kdbx without prompt) |
| `--no-password` | Keyfile-only database (no master password) |

## Security Notes

- Secrets are fetched at runtime, never written to disk in plaintext
- Case-sensitive env var names (supports `TF_VAR_` for Terraform)
- Set `chmod 600` on `.kdbx` and keyfile

## When to Use This

- **Load secrets for a command** — API keys, passwords, tokens from KeePass
- **Replace .env files** — when plaintext secrets are a security concern
- **CI/CD pipelines** — `KEEENV_PASSWORD` for non-interactive auth
- **Inject secrets into scripts** — `keeenv run` for any command
- **Manage credentials across projects** — single KeePass DB, per-project `.keeenv`
