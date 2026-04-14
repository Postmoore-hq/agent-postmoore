# Postmoore Agent Skills — CLAUDE.md

## What this repo is
Open source agent skill for Postmoore (postmoo.re) — a social media scheduling
app.
This repo lives at github.com/Postmoore-hq/agent-postmoore and is a standalone
package that gives AI agents (OpenClaw, Claude Code, Cursor, etc.) the ability
to post to 7 social platforms via the Postmoore REST API.

This is a marketing funnel: anyone who installs the skill must sign up at
postmoo.re to get a `pm_live_...` API key. Main app is private, this repo is
MIT open source.

## Inspired by
github.com/post-bridge-hq/agent-mode — study their structure before writing
anything.

## File structure to build
agent-postmoore/
├── skills/
│   └── postmoore/
│       ├── SKILL.md              ← natural language agent instructions
│       └── scripts/
│           └── postmoore.js      ← zero-dep Node.js CLI wrapping the REST API
├── .claude-plugin/               ← Claude Code plugin config
├── package.json
├── README.md
└── LICENSE (MIT)

## PostMoore REST API
- Base URL: https://postmoo.re/api/v1/
- Auth: Bearer token header → `Authorization: Bearer pm_live_...`
- Key endpoints:
  - GET  /accounts
  - GET  /accounts/:id
  - POST /posts  (body: { contentType, accounts[], schedule, text, media[] })
  - GET  /posts  (?status=pending|scheduled|published|draft)
  - GET  /posts/:id
  - DELETE /posts/:id
  - POST /media (multipart file upload)

## contentType values (POST /posts)
Two values only:
- `"text"` — text-only post, requires `text` field
- `"media"` — image/video post, requires `media[]` array (each object carries file type info: image, video, gif)

## postmoore.js CLI commands to implement
- setup --key pm_live_... [--local]   → store in ~/.config/postmoore/config.json (or .postmoore/config.json if --local)
- accounts                            → GET /accounts
- post --text "..." --accounts 1,2,3 [--schedule ISO] [--draft] [--media id1,id2]
- posts [--status filter]             → GET /posts
- posts:get --id X                    → GET /posts/:id
- posts:delete --id X                 → DELETE /posts/:id
- upload --file ./path                → POST /media
- help

## API key lookup order
1. `POSTMOORE_API_KEY` env var
2. `.postmoore/config.json` — local, relative to cwd (used when `--local` passed to setup)
3. `~/.config/postmoore/config.json` — global default

The `--local` flag on `setup` is useful for per-project keys; global is the
default for personal use.

## Key implementation rules
- Zero external dependencies (pure Node.js built-ins only)
- All output as JSON
- Errors in red via console.error, exit code 1

## Platforms supported (7)
Instagram, Threads, Facebook, YouTube, TikTok, Bluesky, LinkedIn

## SKILL.md format (ClawHub)
YAML frontmatter + markdown body (natural language agent instructions).
The body tells the agent to use the CLI commands above.
Frontmatter must declare: name, description, version, emoji, homepage,
metadata.openclaw.requires.env: [POSTMOORE_API_KEY]
