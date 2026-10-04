---
name: telegram-channel-report
description: Use when the user asks how their Telegram channel is doing — growth, joins and leaves, which posts went out or failed, who the new subscribers are — e.g. "weekly report for my channel", "why did I lose subscribers", "what did we publish this month".
---

# Telegram channel report

Read-only. This skill never creates or changes posts.

## Gather

1. `list_channels` — let the user pick if there are several.
2. `get_channel_stats(channel_id, days=N)` — N from the question, 7 by default, at most 90.
3. `list_posts(channel_id=..., limit=50)` — what was published in the same window. Use `status="failed"` to find delivery problems.
4. Only if the user asks about people: `list_subscribers(channel_id, ...)` / `get_subscriber(...)`.

## Subscriber privacy

Money fields (total spent, lifetime value, payments) are hidden by default. Pass `include_spend=true` **only** when the user explicitly asks about spending or revenue from subscribers — never to answer a general "who are my subscribers" question. Don't list individual people when a count answers the question.

## Write the report

- Lead with the numbers: subscribers now, net change, joins, leaves for the period.
- A short day-by-day view only where something stands out (spike or drop), with the posts published that day next to it. Say "coincides with", not "caused by" — the data shows timing, not cause.
- Failed or partially published posts with the reason, and offer `retry_post_delivery`.
- End with 1–3 concrete suggestions tied to what you saw. No generic growth advice.

Don't present numbers you didn't get from the tools. If a channel has no data for the period, say so.
