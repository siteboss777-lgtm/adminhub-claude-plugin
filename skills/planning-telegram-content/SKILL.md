---
name: planning-telegram-content
description: Plans a series of Telegram channel posts from the channel's recent posts and subscriber stats, then saves the approved posts in AdminHub as drafts or scheduled posts. Use when the user asks for a content plan, a week of posts, or several posts at once for their Telegram channel.
---

# Planning Telegram content

The tools named here are on the AdminHub connector (MCP server `adminhub`). Every write follows the `publishing-telegram-posts` skill.

## 1. Read the channel

- `get_workspace`: time zone and MCP posts left this month (`mcp_posts_limit` minus `mcp_posts_used`; `null` limit means no limit).
- `list_channels`: pick the channel with the user; it needs `bot_is_admin` and `is_active`.
- `list_posts(channel_id=..., limit=20)`, then `get_post` on 3–5 recent posts: topics, voice, length, formatting, posting days and times.
- `get_channel_stats(channel_id, days=30)`: joins and leaves by day. Many leaves on the day of a post is a hint, not proof.
- `list_posts(status="scheduled")`: don't double-book what is already queued.

## 2. Propose the plan

Ask only for what you can't infer: how many posts, which dates, fixed events such as a launch or a sale. Then show:

| # | Date and time (zone) | Topic | Format | Why |
|---|---|---|---|---|

- Keep the channel's current rhythm unless the user asks to change it.
- Never invent prices, dates, offers or facts. Leave `[placeholder]` for anything the user must supply.
- Keep the batch within the MCP posts left this month.

## 3. Save

After the user approves the plan, write every text and show them together. On an explicit yes, save each with `create_post(channel_ids=[...], draft=true)`, so each draft can be published later; use `scheduled_at` instead only if the user asked for scheduling. Finish with the list of post ids and their times.
