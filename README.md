# prompt-library

Manage and retrieve reusable prompts from [broomva.tech](https://broomva.tech/prompts) or any compatible prompt repository. Pull prompts by slug, category, or tag; push new prompts or update existing ones; list available prompts with filtering.

This is a [skills.sh](https://skills.sh) agent skill -- it can be installed by any AI agent that supports the skills protocol.

## Features

- **Pull** prompts by slug with optional variable substitution (`{{name}}` syntax)
- **List** available prompts filtered by category, tag, or model
- **Push** new prompts locally (MDX files) or remotely via authenticated API
- **Update** and **delete** prompts with owner/admin scoping
- **Configurable endpoint** -- point to broomva.tech or your own prompt repository

## Quick Start

### Pull a prompt

```bash
python3 scripts/prompt-sync.py pull code-review-agent
```

### List available prompts

```bash
python3 scripts/prompt-sync.py list
```

### Pull with variable substitution

```bash
python3 scripts/prompt-sync.py pull code-review-agent \
  --var language=python \
  --var strictness=strict
```

### Push a prompt remotely

```bash
python3 scripts/prompt-sync.py remote-push \
  --title "My Prompt" \
  --category system-prompts \
  --body "You are a helpful assistant..." \
  --tags "tag1,tag2" \
  --token "$BROOMVA_API_TOKEN"
```

## Commands

| Command | Description |
|---------|-------------|
| `list` | List all prompts (supports `--category`, `--tag`, `--model` filters) |
| `pull <slug>` | Fetch a prompt by slug, print raw content |
| `push` | Write a new prompt MDX file locally |
| `update <slug>` | Update an existing local prompt, optionally bump version |
| `remote-push` | Create a prompt via the remote API (requires auth token) |
| `remote-update <slug>` | Update a prompt remotely (owner or admin only) |
| `remote-delete <slug>` | Soft-delete a prompt remotely (owner or admin only) |

All commands accept `--endpoint URL` to target a different prompt repository. Default: `https://broomva.tech/api/prompts`.

## Authentication

Remote write commands require an API token:

1. Log in at [broomva.tech](https://broomva.tech) via OAuth (Google/GitHub)
2. Get your token: visit `https://broomva.tech/api/auth/api-token`
3. Set it: `export BROOMVA_API_TOKEN="your-token-here"`

Or pass `--token` directly to any remote command.

## API Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/api/prompts` | Public | List prompts |
| `POST` | `/api/prompts` | Required | Create a new prompt |
| `GET` | `/api/prompts/[slug]` | Public | Get prompt by slug |
| `PUT` | `/api/prompts/[slug]` | Owner/Admin | Update a prompt |
| `DELETE` | `/api/prompts/[slug]` | Owner/Admin | Soft-delete a prompt |

## Prompt Schema

Prompts use MDX frontmatter with key fields:

- **category**: `system-prompts`, `agent-instructions`, `templates`, `chains`, `evaluators`
- **version**: Semver string for tracking prompt evolution
- **variables**: Declared template variables with `{{name}}` syntax and defaults
- **model**: Target model (optional)
- **tags**: Searchable tag array

See `references/prompt-schema.md` for the full specification.

## Hosting Your Own

Anyone can host a compatible prompt library:

1. Create a database table for prompts (or use `content/prompts/` with `.mdx` files)
2. Add the API routes: `GET /api/prompts`, `GET /api/prompts/[slug]`, `POST /api/prompts`
3. Use `--endpoint https://your-site.com/api/prompts` with the sync script

## License

[MIT](LICENSE)
