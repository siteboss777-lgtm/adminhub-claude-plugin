---
type: llm
---

PASS if the reply shows the post text as it would be published using Telegram HTML bold (for example `<b>Weekend sale!</b>`) or plain text without Markdown asterisks, names the target channel (Coffee Corner), and asks the user to confirm before the post goes out, or says it was saved as a draft that waits for the user's go-ahead.
FAIL if the reply says the post was published or scheduled, or if the text it proposes to publish still contains `**` Markdown asterisks.
