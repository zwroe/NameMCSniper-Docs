# Authentication

Bearer tokens are only needed for live claims. Safe simulations and most setup checks do not need a Minecraft account.

## How v3.0.0 Uses Tokens

- Safe simulation is the default and does not use a real token.
- Live mode requires at least one bearer token.
- Multiple tokens can be loaded from `tokens.txt`.
- Tokens are preflighted before the claim window.
- Invalid or ineligible tokens are disabled for the current run.
- Tokens are not automatically refreshed; each token must still be valid when the live claim starts.

## Recommended Storage

Create `tokens.txt` beside `config.yaml`:

```text
# One Minecraft bearer token per line.
# Blank lines and comments are ignored.
first-account-token
second-account-token
```

Keep token fields empty in `config.yaml` when using this file:

```yaml
snipe:
  bearer_token: ""
  bearer_tokens: []
```

!!! warning "Token Security"
    Treat bearer tokens like passwords. Do not paste them in screenshots, Discord, GitHub issues, logs, or committed files.

## Testing Tokens

Validate the primary configured token:

```bash
python Main.py test-token
```

Validate all configured tokens loaded from YAML and `tokens.txt`:

```bash
python Main.py check-accounts
```

## Live Safety Lock

Live claims require deliberate action:

1. Configure a valid token.
2. Use `--live`.
3. Set the acknowledgement environment variable exactly.

PowerShell:

```powershell
$env:NAMEMC_SNIPER_LIVE_ACK='I_UNDERSTAND_THIS_CHANGES_A_REAL_ACCOUNT'
python Main.py snipe-at -u "TargetName" -w "9/15/2026 â€¢ 6:06:50 PM" --timezone America/New_York --live
```

Bash:

```bash
NAMEMC_SNIPER_LIVE_ACK=I_UNDERSTAND_THIS_CHANGES_A_REAL_ACCOUNT \
python Main.py snipe-at -u "TargetName" -w "9/15/2026 â€¢ 6:06:50 PM" --timezone America/New_York --live
```

Without both `--live` and the acknowledgement, non-local claim APIs are rejected before an HTTP session is created.

## Troubleshooting

**No bearer tokens configured**

Add one token per line to `tokens.txt`, or set `snipe.bearer_token` in local `config.yaml`.

**Token appears to be a placeholder or too short**

Replace placeholder text with a real token. The validator rejects common placeholder values and very short strings.

**Token expired before the drop**

Get a fresh token before the scheduled run. NameMCSniper does not refresh bearer tokens automatically.

**No Minecraft Java profile found**

Use a token from an account that owns Java Edition and has an existing Java profile.
