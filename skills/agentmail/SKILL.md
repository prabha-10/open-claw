# AgentMail Skill

Send and receive emails using AgentMail.

## Configuration

Requires an API Key from [AgentMail](https://agentmail.to).
Set it via: `openclaw config set skills.entries.agentmail.env.AGENTMAIL_API_KEY <KEY>`

## Tools

### `email_inbox_list`
List all inboxes.
```bash
curl -s -H "Authorization: Bearer $AGENTMAIL_API_KEY" \
  https://api.agentmail.to/v0/inboxes
```

### `email_inbox_create`
Create a new inbox.
- `name`: (Optional) Display name for the inbox.
```bash
# Usage: email_inbox_create "My Project Inbox"
curl -s -X POST -H "Authorization: Bearer $AGENTMAIL_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"name\": \"$1\"}" \
  https://api.agentmail.to/v0/inboxes
```

### `email_send`
Send an email.
- `inbox_id`: The ID of the inbox to send from.
- `to`: Recipient email address.
- `subject`: Subject line.
- `body`: Plain text body.
```bash
# Usage: email_send <inbox_id> <to> <subject> <body>
curl -s -X POST -H "Authorization: Bearer $AGENTMAIL_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"to\": [\"$2\"], \"subject\": \"$3\", \"text\": \"$4\"}" \
  https://api.agentmail.to/v0/inboxes/$1/messages/send
```

### `email_list_messages`
List messages in an inbox.
- `inbox_id`: The ID of the inbox.
```bash
# Usage: email_list_messages <inbox_id>
curl -s -H "Authorization: Bearer $AGENTMAIL_API_KEY" \
  https://api.agentmail.to/v0/inboxes/$1/messages
```

### `email_reply`
Reply to a specific message.
- `inbox_id`: The inbox ID.
- `message_id`: The message ID to reply to.
- `text`: The reply body.
```bash
# Usage: email_reply <inbox_id> <message_id> <text>
curl -s -X POST -H "Authorization: Bearer $AGENTMAIL_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"text\": \"$3\"}" \
  https://api.agentmail.to/v0/inboxes/$1/messages/$2/reply
```
