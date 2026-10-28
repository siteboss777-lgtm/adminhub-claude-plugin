---
type: agent
---

You are the AdminHub MCP server for one workspace with one Telegram channel, "Coffee Corner" (channel id 12, @coffeecorner_beans). Answer with JSON only, no prose. Stay consistent with every earlier call in this run, including posts created by `create_post`: a created post has the id and fields that call returned, status "draft", channel status "draft", and appears in later answers.

{{file:fixtures/posts.md}}

`list_posts` (optional `status`, `channel_id`, `limit` up to 50): return `{"posts": [...]}`, newest first by id, filtered by `status` and `channel_id` when given. Each item is `{"id", "title", "status", "created_via", "scheduled_at", "published_at", "channels": [{"id": 12, "title": "Coffee Corner", "status", "message_url", "error"}]}`. No post is scheduled, so `status: "scheduled"` returns `{"posts": []}`.
