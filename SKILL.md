---
name: prompt-library
description: >
  Manage and retrieve reusable prompts from broomva.tech or any compatible prompt repository.
  Pull prompts by slug, category, or tag; push new prompts or update existing ones; list
  available prompts with filtering. Use when: (1) user asks for a prompt, system instruction,
  or agent directive, (2) user wants to save a prompt for reuse, (3) user mentions "prompt
  library", "prompt repo", "save this prompt", "get prompt", "pull prompt", (4) agent needs
  a reusable system prompt for a task like code review, research, or architecture analysis,
  (5) user says "use the X prompt" referring to a named prompt. Default distribution endpoint:
  https://broomva.tech/api/prompts
---

# Prompt Library

Manage versioned, parameterized prompts stored as MDX files and distributed via a JSON API.

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
python3 scripts/prompt-sync.py pull code-review-agent --var language=python --var strictness=strict
```

## Commands

| Command | Description |
|---------|-------------|
| `list` | List all prompts (supports `--category`, `--tag`, `--model` filters) |
| `pull <slug>` | Fetch a prompt by slug, print raw content |
| `push` | Write a new prompt MDX file to local `content/prompts/` |
| `update <slug>` | Update an existing prompt, optionally bump version |

All commands accept `--endpoint URL` to target a different prompt repository. Default: `https://broomva.tech/api/prompts`.

## Prompt Schema

See `references/prompt-schema.md` for the full frontmatter specification.

Key fields:
- `category`: system-prompts, agent-instructions, templates, chains, evaluators
- `version`: Semver string for tracking prompt evolution
- `variables`: Declared template variables with `{{name}}` syntax and defaults
- `model`: Target model (optional)
- `tags`: Searchable tag array

## Category Taxonomy

See `references/categories.md` for the full taxonomy.

## For Other Repositories

Anyone can host their own prompt library:

1. Create `content/prompts/` with `.mdx` files following the schema
2. Add the two API routes: `GET /api/prompts` and `GET /api/prompts/[slug]`
3. Use `--endpoint https://your-site.com/api/prompts` with the sync script

The schema and API contract are the standard. The endpoint is configurable.
