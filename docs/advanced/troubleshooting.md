# Troubleshooting

Use this guide for the current v3.0.0 CLI.

## Quick Checks

```bash
python Main.py --version
python Main.py --help
python Main.py config-validate
python Main.py simulate
```

Expected version:

```text
NameMCSniper, version 3.0.0
```

## Installation Issues

### Python Version Error

NameMCSniper requires Python 3.10 or newer.

Check:

```bash
python --version
```

Recreate `.venv` with Python 3.10+ if needed.

### Module Not Found

Install dependencies inside the virtual environment:

Windows:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Linux/macOS:

```bash
.venv/bin/python -m pip install -r requirements.txt
```

## Config Issues

### Unknown Config Key

The config model rejects obsolete or misspelled keys. Create a fresh config and move current values into it:

```bash
python Main.py config-create --config fresh-config.yaml
```

### No Bearer Tokens Configured

Dry runs do not need tokens. Live claims do.

Add tokens to `tokens.txt`:

```text
first-account-token
second-account-token
```

Then run:

```bash
python Main.py check-accounts
```

### Timezone Required

Scheduled live claims require an explicit timezone:

```bash
python Main.py snipe-at -u "TargetName" -w "9/15/2026 â€¢ 6:06:50 PM" --timezone America/New_York --live
```

Supported timezone forms:

- `America/New_York`
- `UTC`
- `-04:00`
- Aliases such as `Eastern`, `Central`, `Mountain`, and `Pacific`

## Time Parsing Issues

### Expected NameMC Time Format

Use:

```text
M/D/YYYY â€¢ H:MM:SS AM/PM
```

Example:

```text
9/15/2026 â€¢ 6:06:50 PM
```

### Time Is in the Past

Use a future drop time. The example date `September 15, 2026` is only a placeholder.

### DST Error

The selected local time may be ambiguous or nonexistent because of daylight saving time. Use a different exact time, `UTC`, or a numeric offset if you know the intended offset.

## Live Mode Issues

### Live Claim Does Not Start

Live mode needs both:

```bash
--live
```

and:

```text
NAMEMC_SNIPER_LIVE_ACK=I_UNDERSTAND_THIS_CHANGES_A_REAL_ACCOUNT
```

Easy mode sets the acknowledgement internally after confirmation for that guided run.

### Token Expired

Bearer tokens are not refreshed automatically. Get a fresh token before the live claim window.

### No Java Profile Found

Use a token from an account that owns Java Edition and has an existing Java profile.

## Proxy Issues

### SOCKS Proxy Rejected

Current proxy support is HTTP(S) only.

### Proxy List Empty

If `proxy.enabled: true`, provide proxies in `proxies.txt` or `proxy.proxies`.

### No Working Proxies

Run:

```bash
python Main.py check-proxies
```

Remove dead, slow, or blocked proxies. A direct connection may be better.

## Discord Issues

### No Webhook Notification

- Confirm `discord.enabled: true`.
- Confirm the webhook URL is HTTPS and belongs to Discord.
- Send a manual webhook test.
- Remember that safe dry runs suppress external services.

### Bot Settings Do Nothing

Discord bots are not supported in v3.0.0. Use webhooks only.

## Long Scheduled Runs

### No Output for a While

Long waits log periodically:

- Every five minutes during long waits
- Every minute during the final hour
- Every second during the final minute

### Clock Source Fails

If every external clock source fails, the app logs the issue and continues with the local system clock.

### Priority Warning

If the OS refuses high process priority, the run continues without the priority boost.

## Logs

Run logs are written under `logs/` with names like:

```text
logs/namemc_sniper_YYYYMMDD_HHMMSS.log
```

Search for errors:

```bash
grep -i "error" logs/namemc_sniper_*.log
```

PowerShell:

```powershell
Select-String -Path logs\namemc_sniper_*.log -Pattern "error"
```

## Getting Help

When asking for help, include:

- NameMCSniper version
- Python version
- Operating system
- Command used
- Sanitized config
- Relevant logs with tokens, webhook URLs, and proxy credentials removed
