# Discord Setup

NameMCSniper v3.0.0 supports Discord notifications through webhooks only.

Discord is optional. Safe dry runs do not contact Discord, and easy mode disables Discord for the run so notifications stay out of the precision timing path.

## What Gets Sent

When Discord is enabled for an eligible run, the notifier can send:

- Drop countdown updates
- Start notifications
- Success or failure results
- Error notifications
- General status updates

## Create a Webhook

1. Open Discord and go to your server.
2. Right-click the channel for notifications.
3. Select **Edit Channel**.
4. Open **Integrations**.
5. Create a webhook.
6. Copy the webhook URL.

## Configure Discord

In local `config.yaml`:

```yaml
discord:
  enabled: true
  webhook_url: "https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN"
  mention_role_id: ""
  embed_color: 65280
```

Optional role mention:

```yaml
discord:
  mention_role_id: "123456789012345678"
```

Use only the numeric role ID.

## Validate the Config

```bash
python Main.py config-validate
```

For live mode, the validator requires an HTTPS webhook URL on `discord.com` or `discordapp.com` when Discord is enabled.

## Test the Webhook Manually

```bash
curl -X POST "YOUR_WEBHOOK_URL" \
  -H "Content-Type: application/json" \
  -d '{"content": "Test message from NameMCSniper"}'
```

PowerShell:

```powershell
Invoke-RestMethod `
  -Method Post `
  -Uri "YOUR_WEBHOOK_URL" `
  -ContentType "application/json" `
  -Body '{"content":"Test message from NameMCSniper"}'
```

## Embed Colors

Discord embed colors are decimal RGB values.

| Color | Decimal | Hex |
|-------|---------|-----|
| Green | `65280` | `#00FF00` |
| Red | `16711680` | `#FF0000` |
| Orange | `16753920` | `#FFA500` |
| Blue | `39321` | `#009999` |

## Rate Limits

The webhook sender uses bounded timeouts and handles Discord `429` rate-limit responses by skipping that notification instead of blocking the main run.

## Security

- Do not share webhook URLs.
- Regenerate a webhook if it is exposed.
- Use private channels for sensitive notifications.
- Avoid logging or posting bearer tokens, proxy credentials, or full config files.

## Troubleshooting

**No notification appears**

- Confirm `discord.enabled` is `true`.
- Confirm the webhook URL is complete.
- Send a manual test request.
- Check the channel still exists and the webhook was not deleted.

**Config validation fails**

- Use an HTTPS Discord webhook URL.
- Keep `mention_role_id` numeric.

**Dry run did not send Discord messages**

That is expected. Safe dry runs suppress external services.
