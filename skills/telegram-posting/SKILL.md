---
name: telegram-posting
description: Use when the user wants to write, publish, schedule, edit, retry or cancel a post in their Telegram channel through AdminHub — e.g. "post this to my channel", "schedule this for tomorrow 10am", "fix the typo in yesterday's post", "add this image to the draft".
---

# Posting to a Telegram channel with AdminHub

The AdminHub MCP tools publish through the user's **own Telegram bot**, which must be an admin of the channel. Nothing is sent from the user's personal account.

## Before the first write in a conversation

1. Call `get_workspace` — it gives the plan, the bot status, the workspace time zone, this month's MCP post usage, and `owner_dm_open` (needed for images, see below).
2. Call `list_channels`. A channel can receive posts only if **both** `bot_is_admin` and `is_active` are true. If none qualify, tell the user to add their bot as an admin of the channel in Telegram — do not try other ids.

## Confirm before anything goes public

Posts are seen by real subscribers and cannot be unsent through these tools. Before `create_post` without `draft=true`, `publish_post` or `edit_published_post`:

- Show the user the final text, the target channel(s) and the time (or "right now").
- Wait for an explicit yes.

When in doubt, create with `draft=true` and let the user publish from AdminHub or tell you to `publish_post`.

## Writing the text

- Text is Telegram HTML: `<b>`, `<i>`, `<u>`, `<s>`, `<a href="...">`, `<code>`, `<pre>`, `<blockquote>`, `<tg-spoiler>`. Use plain line breaks for paragraphs.
- Markdown is **not** converted — `**bold**` would be published with the asterisks. Headings and lists are flattened (to a bold line and `•` bullets), so write them that way yourself.
- Escape literal `<`, `>` and `&` in the text as `&lt;`, `&gt;`, `&amp;`.
- The cap is 4096 visible characters (links count by their visible text, not the URL).
- With an image attached, the first 1024 characters ride under the image as the caption and the rest is sent as follow-up messages. Keep image posts under 1024 characters if the user wants a single message.
- Write in the channel's own language and voice — read a few recent posts with `list_posts` / `get_post` first if you don't know it.
- No hashtags unless the channel already uses them.

## Scheduling

- `scheduled_at` must be ISO 8601 **with an offset**, e.g. `2026-10-06T10:00:00+03:00`. A time without a zone is rejected, a past time is rejected.
- "Tomorrow at 10" means 10:00 in the user's time zone — take it from `get_workspace` and say which zone you used.
- Omit `scheduled_at` to send immediately.

## Images

`add_post_image(post_id, image_url)` attaches a picture from a public URL to a draft, scheduled or failed post (up to 10 per post).

- Telegram gives a bot no way to upload by URL without sending the file somewhere first, so AdminHub briefly sends the image to the owner's private chat with their bot and deletes it. Tell the user they will see a photo flash by from their bot.
- This needs `owner_dm_open: true`. If it is false, ask the owner to open their bot in Telegram and press Start, then retry.

## Changing things after the fact

| Situation | Tool |
|---|---|
| Draft / scheduled / failed post: change text, title or time | `update_post` (only passed fields change) |
| Already published: fix the text | `edit_published_post` — text only; check `edit_results` per channel |
| Delivery failed in some channel | `retry_post_delivery` (optionally one `channel_id`), then check `get_post` after a minute |
| Remove a draft / scheduled / failed post | `cancel_post` — this **deletes** the post; published posts cannot be removed this way |

Moving a scheduled time to "send now" is not possible through `update_post`; cancel and create a new post instead.

## Limits and errors

- The Free plan allows 30 MCP posts per calendar month; `get_workspace` shows the usage. If the limit is hit, say so plainly and mention that Pro removes it — don't retry.
- Tool errors carry a code and a human-readable message. Relay the message; don't guess around it (for example, never switch to another channel id the user did not ask for).
