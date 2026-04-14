# Postmoore Agent Skill

Give AI agents the ability to post to **7 social platforms** via the [Postmoore](https://postmoo.re) REST API.

Works with Claude Code, OpenClaw, Cursor, and any agent that can run shell commands.

**Platforms:** Instagram · Threads · Facebook · YouTube · TikTok · Bluesky · LinkedIn

---

## Requirements

- Node.js 16+
- A Postmoore account and API key — sign up at **[postmoo.re](https://postmoo.re)**

---

## Install

```bash
npm install -g @postmoore/agent-postmoore
```

Or run without installing:

```bash
npx @postmoore/agent-postmoore help
```

---

## Setup

Get your API key from [postmoo.re](https://postmoo.re/profile), then run:

```bash
# Store globally (default — recommended for personal use)
postmoore setup --key pm_live_YOUR_KEY

# Store per-project (useful for team/CI environments)
postmoore setup --key pm_live_YOUR_KEY --local
```

Or set the environment variable:

```bash
export POSTMOORE_API_KEY=pm_live_YOUR_KEY
```

**Key lookup order:**

1. `POSTMOORE_API_KEY` env var
2. `.postmoore/config.json` in current directory (`--local`)
3. `~/.config/postmoore/config.json` (global)

---

## Commands

### `accounts`

List all social accounts connected to your Postmoore workspace.

```bash
postmoore accounts
```

### `post`

Create a post on one or more platforms.

```bash
# Text post
postmoore post --text "Hello world!" --accounts 1,2,3

# Scheduled post (ISO 8601)
postmoore post --text "Coming soon" --accounts 1 --schedule 2026-05-01T09:00:00Z

# Save as draft
postmoore post --text "Draft" --accounts 1,2 --draft

# Media post (upload first, then post)
postmoore upload --file ./photo.jpg
postmoore post --accounts 1,2 --media MEDIA_ID
```

### `posts`

List posts, optionally filtered by status.

```bash
postmoore posts
postmoore posts --status scheduled
postmoore posts --status published
postmoore posts --status draft
postmoore posts --status pending
```

### `posts:get`

Get a single post by ID.

```bash
postmoore posts:get --id 42
```

### `posts:delete`

Delete a post by ID.

```bash
postmoore posts:delete --id 42
```

### `upload`

Upload a media file. Returns a media ID to use in `post --media`.

```bash
postmoore upload --file ./video.mp4
```

### `help`

```bash
postmoore help
```

---

## For AI Agents

This package includes a `SKILL.md` at `skills/postmoore/SKILL.md` with natural-language instructions for agents. It is compatible with:

- **Claude Code** — via `.claude-plugin/`
- **OpenClaw / ClawHub** — SKILL.md with YAML frontmatter
- **Cursor** — run CLI commands via terminal tool

---

## License

MIT — see [LICENSE](./LICENSE)
