---
name: telegram-content-plan
description: Use when the user wants a content plan or a batch of posts for their Telegram channel — e.g. "plan next week's posts", "write 5 posts for my channel and schedule them", "fill my channel for the holidays".
---

# Planning a week of Telegram posts

Goal: a short, realistic plan the user approves, then saved in AdminHub as drafts or scheduled posts. Follow the `telegram-posting` skill for every write.

## 1. Learn the channel (read-only)

- `get_workspace` — time zone, plan, remaining MCP posts this month.
- `list_channels` — pick the channel with the user; it must have `bot_is_admin` and `is_active`.
- `list_posts(channel_id=..., limit=20)` and `get_post` on 3–5 recent ones — topics, tone, length, formatting, posting rhythm.
- `get_channel_stats(channel_id, days=30)` — joins and leaves by day. Note days with unusual leaves next to the posts published then; that is a hint, not proof.
- `list_posts(status="scheduled")` — don't plan on top of what is already queued.

## 2. Propose the plan

Ask only what you can't infer: how many posts, which days, any fixed dates (launch, sale, event). Then show a table:

| # | Date & time (zone) | Topic | Format | Why |
|---|---|---|---|---|

- Match the cadence the channel already has; don't double it unasked.
- Mix formats the channel already uses. Don't invent offers, prices, dates or facts — leave `[placeholders]` for anything the user must supply.
- On the Free plan, keep the batch within the remaining monthly MCP posts.

## 3. Write and save

After the user approves the plan, write the full texts and show them all at once. Then, on an explicit yes:

- Default: `create_post(..., draft=true)` for every post, so nothing goes out unseen.
- Only if the user asked for scheduling: `create_post` with `scheduled_at` per row.
- Report back a list of post ids with their times, and remind the user that `cancel_post` deletes a queued post and `update_post` changes its text or time.
