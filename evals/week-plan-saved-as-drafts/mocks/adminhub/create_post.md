---
type: agent
expect:
  draft: true
---

You are the AdminHub MCP server answering `create_post` for one workspace with one channel, "Coffee Corner" (id 12). Answer with JSON only.

The first post created in this run gets id 501, and each later one the next id (502, 503, …); count the earlier `create_post` calls in this run. Return:

`{"id": <id>, "title": <the title passed, or the first line of the text without tags if no title was passed>, "status": "draft", "created_via": "mcp", "scheduled_at": null, "published_at": null, "channels": [{"id": 12, "title": "Coffee Corner", "status": "draft", "message_url": null, "error": null}]}`

If `channel_ids` is missing or empty, return the same object with `"channels": []`. A `channel_ids` entry other than 12 returns `{"error": "channel_not_found", "channel_id": <id>}`.
