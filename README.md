# AdminHub for Telegram

Run your Telegram channel from Claude. Ask Claude to write and schedule a post, fix a typo in one that's already out, plan next week's content, or explain how the channel grew. AdminHub publishes through **your own Telegram bot**, never through your personal account.

## What's inside

- **AdminHub connector**: AdminHub's hosted MCP server at `https://mcp.adminhub.tools/mcp`. You sign in with Telegram and approve the permissions Claude asks for.
- **Skills** that teach Claude the workflow:
  - `publishing-telegram-posts`: checks the bot can post to the channel, writes Telegram HTML, reads times in your time zone, and confirms before anything goes public.
  - `planning-telegram-content`: reads your recent posts and growth, proposes a plan, and saves the approved posts as drafts or scheduled posts.
  - `analyzing-telegram-channels`: growth, joins and leaves by day, and failed deliveries, with subscribers' spending kept private unless you ask for it.

## Before you start

1. Open [@tg_adminhub_bot](https://telegram.me/tg_adminhub_bot) in Telegram and set up your workspace.
2. Connect your own bot in AdminHub and add your channel. The bot must be an admin of the channel.

Plans and limits for AI agents are listed at [adminhub.tools/mcp](https://adminhub.tools/mcp/).

## Install

**Claude apps (web, desktop, Cowork):** in **Customize > Plugins**, find AdminHub for Telegram in **Discover** once it's listed, or choose **Add > Add marketplace** and enter `siteboss777-lgtm/adminhub-claude-plugin`. Then open the plugin's **Connectors** tab and connect AdminHub.

**Claude Code:**

```
/plugin marketplace add siteboss777-lgtm/adminhub-claude-plugin
/plugin install adminhub@adminhub
```

Then run `/mcp`, choose `adminhub` and sign in with Telegram.

## Try it

- "Check my channel and plan next week's posts. Save them as drafts."
- "Schedule this announcement for Monday at 10:00 my time: we open on Tuesday."
- "How did my channel do this week, and did any post fail to go out?"

## Tools and permissions

The sign-in page lists each permission before you approve it, and every tool checks its own.

| Permission | Tools |
|---|---|
| Read channels and posts | `get_workspace`, `list_channels`, `get_channel_stats`, `list_posts`, `get_post` |
| Write posts | `create_post`, `publish_post`, `update_post`, `edit_published_post`, `add_post_image`, `retry_post_delivery`, `cancel_post` |
| Read subscribers | `list_subscribers`, `get_subscriber` |

## Data

- The plugin is instructions and a connector address. It runs no code on your computer and stores nothing itself.
- Everything goes through the AdminHub connector above. When you ask Claude to post, the text, title, time and target channel are sent to AdminHub, saved as a post in your workspace and published to Telegram by your bot.
- When you ask for an image, AdminHub's server downloads it from the link Claude passes and sends it once through your private chat with your bot to attach it to the post.
- Read tools return what AdminHub already holds for your workspace: channels, posts, subscriber counts, channel revenue totals and daily joins and leaves. Subscribers' names, usernames and Telegram IDs come under a separate permission listed on the sign-in page, and a subscriber's spending is returned only when explicitly requested.

[Privacy policy](https://adminhub.tools/privacy/) · [Terms of service](https://adminhub.tools/terms/)

## Support

Setup guide: [adminhub.tools/blog/telegram-mcp-setup](https://adminhub.tools/blog/telegram-mcp-setup/). Questions: [@tg_adminhub_help_bot](https://telegram.me/tg_adminhub_help_bot).

## License

MIT
