# Google Chat Skill

Send messages to Google Chat spaces and reply to threads directly from Claude Code.

## Quick Setup

### Configure Your Space

Create a `config.jsonc` file in this skill directory:

```jsonc
{
  "key": "<api_key>",
  "tokens": {
    "<space_id1>": "<space_token1>", // deployment
    "<space_id2>": "<space_token2>", // sentry-alerts
  },
}
```

**Getting Your Space ID and token:**

1. Open your Google Chat space in a web browser
2. Add a webhook if not available yet
3. Inspect the webhook URL: https://chat.googleapis.com/v1/spaces/<space_id>/messages?key=<api_key>&token=<api_token> to retrieve the api_key, space_id and space_token

### Test It

```bash
python scripts/send_message.py --message "Hello from Claude!"
```

## Usage

### Sending New Messages

Once configured, you can ask Claude to send messages:

- "Send to chat: Task completed successfully"
- "Notify the team about the deployment"
- "Post to Google Chat: Build passed ✅"

**With formatting:**

```bash
python scripts/send_message.py --message "*Important:* Task completed ✅" --formatted
```

### Replying to Threads

Reply to existing threads using the thread name:

```bash
python scripts/reply_message.py --thread-name "spaces/<space_id>/threads/<thread_id>" --message "Tests are now passing"
```

**Thread names** follow the format: `spaces/{SPACE_ID}/threads/{THREAD_ID}`

You can get thread names from:

- Google Chat web interface (from thread URL)
- Previous message responses (API returns thread names)
- User providing the thread link

**With formatting:**

```bash
python scripts/reply_message.py --thread-name "spaces/<space_id>/threads/<thread_id>" --message "*Update:* Task completed ✅" --formatted
```

## Message Formatting

The skill supports rich text formatting with the `--formatted` flag:

- **Bold**: `*bold*`
- _Italic_: `_italic_`
- ~Strikethrough~: `~strikethrough~`
- `Code`: `` `code` ``
- Links: `<https://example.com|Link Text>`
- Emoji: ✅ ⚠️ 🚀
- Lists (bullet and numbered)
- Code blocks
- User mentions

See `references/message_formatting.md` for comprehensive examples and templates.

## Troubleshooting

### "Permission denied" error

Make sure the service account email has been added to your Google Chat space as a member.

### "Space ID not configured"

Check that `config.jsonc` exists and has the correct space ID format.

### "requests library not installed"

Run `pip install requests` to install the required library.

## Examples

**Basic message:**

```bash
python scripts/send_message.py --message "Deployment complete"
```

**Formatted message with emoji:**

```bash
python scripts/send_message.py --message "🚀 *Deployment Complete* - All services are now live" --formatted
```

**Reply to a thread:**

```bash
python scripts/reply_message.py --thread-name "spaces/ABC123/threads/XYZ789" --message "Issue resolved ✅" --formatted
```
