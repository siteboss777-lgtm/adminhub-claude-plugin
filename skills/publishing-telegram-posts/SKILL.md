---
name: publishing-telegram-posts
description: Writes, publishes, schedules and edits posts in a Telegram channel through the AdminHub connector, which sends them from the user's own bot. Use when the user asks to post, schedule, fix, illustrate, retry or delete a post in their Telegram channel.
---

# Publishing Telegram posts

The tools named here are on the AdminHub connector (MCP server `adminhub`).

## Before the first write

1. `get_workspace`: time zone, bot status, MCP post usage (`mcp_posts_used` / `mcp_posts_limit`, where `null` means no limit) and `owner_dm_open`.
2. `list_channels`: only channels with both `bot_is_admin` and `is_active` true can receive posts. If none qualify, tell the user to make their bot an admin of the channel. Never switch to a channel the user didn't pick.

If a tool answers `bot_required`, give the user the link from the error and stop.

## Confirm before anything goes public

A post reaches real subscribers and can't be unsent. Before `create_post` without `draft=true`, `publish_post` or `edit_published_post`, show the final text, the channel and the time ("now", or the exact time with its zone), then wait for a yes. If the user already gave the exact text, channel and time, one short confirmation is enough. When unsure, save with `draft=true`.

## Text

- Telegram HTML only: `<b>`, `<i>`, `<u>`, `<s>`, `<a href="...">`, `<code>`, `<pre>`, `<blockquote>`, `<tg-spoiler>`. Convert the user's Markdown (`**bold**` → `<b>bold</b>`), because Markdown is published as typed, asterisks included.
- Escape literal `<`, `>` and `&` as `&lt;`, `&gt;` and `&amp;`.
- At most 4096 visible characters. A post with an image gets a much shorter caption (1024 characters), and longer text is split into follow-up messages, so keep image posts short when the user wants one message.
- Match the channel's language and voice; if unknown, read a few recent posts with `list_posts` and `get_post`. Don't add hashtags or emoji the channel doesn't use.

## Time

`scheduled_at` is ISO 8601 with an offset, e.g. `2026-10-06T10:00:00+03:00`. Read relative times ("tomorrow at 10") in the workspace time zone and say which zone you used. Omit `scheduled_at` to send now.

## After creating

Delivery is queued and happens within about a minute. Report the post id; if the user is waiting for it, check `get_post` for each channel's status and `error`.

## Changing a post

- Draft, scheduled or failed: `update_post` changes only the fields you pass. To send a scheduled post now, cancel it and create a new one.
- Published: `edit_published_post` changes the text only; report `edit_results` per channel.
- Failed delivery: `retry_post_delivery`, then check `get_post`.
- `cancel_post` permanently deletes a draft, scheduled or failed post; say so before calling it.
- Images: `add_post_image` needs `owner_dm_open: true`. Warn the user that the photo briefly appears in their own chat with the bot.

## Limits

If `mcp_posts_limit` is a number and `mcp_posts_used` has reached it, tell the user this month's MCP post limit is used up and stop. Don't retry or work around it.
