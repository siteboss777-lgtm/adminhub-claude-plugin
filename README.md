# AdminHub for Telegram — Claude plugin

Run a Telegram channel from Claude: write, schedule and edit posts, read channel stats and subscribers. Posts go out through **your own Telegram bot**, not your personal account.

## What's inside

- **MCP connector** — AdminHub's hosted MCP server (`https://backend-git-production-cb93.up.railway.app/mcp`), 14 tools, OAuth sign-in with Telegram.
- **Skills**
  - `telegram-posting` — safe posting: checks the bot is admin, confirms before publishing, Telegram HTML, time zones, images.
  - `telegram-content-plan` — reads your channel and turns a week's plan into drafts or scheduled posts.
  - `telegram-channel-report` — growth, joins/leaves, failed deliveries, with subscriber spending kept private by default.

## Requirements

1. An AdminHub account — open [@tg_adminhub_bot](https://t.me/tg_adminhub_bot) in Telegram.
2. Your own bot connected in AdminHub and added as an admin of your channel.

The Free plan includes one MCP connection and 30 posts a month through MCP; Pro removes the limits.

## Install in Claude Code

```
/plugin marketplace add siteboss777-lgtm/adminhub-claude-plugin
/plugin install adminhub@adminhub
```

Then run `/mcp`, pick `adminhub` and sign in with Telegram on the consent page.

## Tools

| Read (`channel:read`) | Write (`posts:write`) | Audience (`audience:read`) |
|---|---|---|
| `get_workspace`, `list_channels`, `get_channel_stats`, `list_posts`, `get_post` | `create_post`, `publish_post`, `update_post`, `edit_published_post`, `add_post_image`, `retry_post_delivery`, `cancel_post` | `list_subscribers`, `get_subscriber` |

You choose which permissions to grant on the consent page.

## Example prompts

- "Check my channel and plan next week's posts — save them as drafts."
- "Schedule this announcement for Monday 10:00 my time and pin it."
- "Weekly report for my channel: growth, leaves and any failed posts."

Docs: https://adminhub.tools/mcp/ · Support: [@tg_adminhub_bot](https://t.me/tg_adminhub_bot)
