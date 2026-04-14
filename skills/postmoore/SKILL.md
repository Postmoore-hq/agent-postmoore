---
name: postmoore
description: Post to Instagram, Threads, Facebook, YouTube, TikTok, Bluesky, and LinkedIn via the Postmoore API
version: 1.0.0
emoji: 🚀
homepage: https://github.com/Postmoore-hq/agent-postmoore
metadata: {"openclaw": {"requires": {"env": ["POSTMOORE_API_KEY"]}, "primaryEnv": "POSTMOORE_API_KEY"}}
---

# Postmoore Agent Skill

Post content to 7 social media platforms — Instagram, Threads, Facebook, YouTube, TikTok, Bluesky, and LinkedIn — using the `postmoore` CLI.

## Prerequisites

You need a Postmoore API key (`pm_live_...`). Get one by signing up at https://postmoo.re.

Set it via environment variable or run setup once:

```bash
postmoore setup --key pm_live_YOUR_KEY
```

Use `--local` to store the key in `.postmoore/config.json` relative to the current project instead of globally.

## Usage

### List connected accounts

Before posting, list the accounts connected to your Postmoore workspace. Use the `id` values when posting.

```bash
postmoore accounts
```

### Create a text post

```bash
postmoore post --text "Hello world!" --accounts 1,2,3
```

### Schedule a post

Provide an ISO 8601 datetime for `--schedule`:

```bash
postmoore post --text "Scheduled post" --accounts 1 --schedule 2026-05-01T09:00:00Z
```

### Save as draft

```bash
postmoore post --text "Draft content" --accounts 1,2 --draft
```

### Upload media and create a media post

First upload the file to get a media ID, then use it in a post:

```bash
postmoore upload --file ./image.jpg
# returns { "id": "abc123", ... }

postmoore post --accounts 1,2 --media abc123
```

Multiple media IDs are comma-separated: `--media id1,id2`.

### List posts

```bash
postmoore posts
postmoore posts --status scheduled
postmoore posts --status published
postmoore posts --status draft
postmoore posts --status pending
```

### Get a single post

```bash
postmoore posts:get --id 42
```

### Delete a post

```bash
postmoore posts:delete --id 42
```

### Help

```bash
postmoore help
```

## API key lookup order

1. `POSTMOORE_API_KEY` environment variable
2. `.postmoore/config.json` in the current directory (set via `setup --local`)
3. `~/.config/postmoore/config.json` (global, set via `setup`)

## All output is JSON

Every command prints JSON to stdout. Errors print to stderr in red and exit with code 1.
