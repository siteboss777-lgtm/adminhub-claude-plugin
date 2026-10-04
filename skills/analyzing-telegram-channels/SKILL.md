---
name: analyzing-telegram-channels
description: Reports how a Telegram channel connected to AdminHub is doing, covering subscriber growth, joins and leaves by day, and published and failed posts, using read-only tools and keeping subscribers' spending private. Use when the user asks how their channel is doing, why subscribers left, what was published, or who joined recently.
---

# Analyzing a Telegram channel

Read-only: this skill never creates, edits or deletes posts. The tools named here are on the AdminHub connector (MCP server `adminhub`).

## Gather

1. `list_channels`; ask which channel if there are several.
2. `get_channel_stats(channel_id, days=N)`: N from the question, 7 by default, at most 90.
3. `list_posts(channel_id=..., limit=50)`, keeping the posts published in the same period. Use `status="failed"` to find delivery problems.
4. Only if the user asks about people: `list_subscribers` or `get_subscriber`. Pass `include_spend=true` only when the user asks about someone's purchases or spending. Prefer counts to listing individuals.

## Report

- Lead with the numbers for the period: subscribers now, net change, joins, leaves.
- Call out only the days that stand out, with the posts published that day. Say "coincides with", not "caused by".
- List failed or partly published posts with their `error`, and offer `retry_post_delivery`.
- End with one to three suggestions tied to what the data shows.

Use only numbers the tools returned. If a period has no data, say so.
