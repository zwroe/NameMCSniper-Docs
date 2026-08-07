# Basic Usage

This page covers the normal v3.0.0 workflow: start with safe simulation, then only unlock live mode when you intentionally want a real claim.

## Start With Easy Mode

```bash
python Main.py easy
```

Easy mode asks:

1. Target Minecraft username.
2. Safe simulation or real claim.
3. Run now, 30 seconds from now, or at an exact NameMC drop time.

For exact drop times, common US timezones are presented as choices. You can also enter an IANA timezone such as `America/Phoenix`, `Europe/London`, or `UTC`.

!!! tip "Recommended First Run"
    Choose safe simulation. It cannot change an account and keeps proxies, Discord, Minecraft, NTP, and public time APIs out of the run.

## Run an Immediate Simulation

```bash
python Main.py simulate
```

Try different simulated outcomes:

```bash
python Main.py simulate --scenario success
python Main.py simulate --scenario rate_limited_then_success --success-after 4
python Main.py simulate --scenario auth_error
python Main.py simulate --scenario timeout
```

Supported scenarios:

- `success`
- `taken`
- `taken_then_success`
- `rate_limited_then_success`
- `auth_error`
- `server_error`
- `timeout`

## Run a Scheduled Dry Run

Scheduled commands are dry runs unless you pass `--live`.

```bash
python Main.py snipe-at -u "TargetName" -w "9/15/2026 â€¢ 6:06:50 PM" --timezone America/New_York
```

The NameMC time format is:

```text
M/D/YYYY â€¢ H:MM:SS AM/PM
```

Regular colons are supported. The older mathematical-colon variant is also normalized.

## Run Fallback Windows

Fallback mode tries multiple scheduled windows in order. It is also a dry run unless `--live` is supplied.

```bash
python Main.py snipe-fallback \
  -u "TargetName" \
  -t "9/15/2026 â€¢ 6:06:50 PM" \
  -t "9/15/2026 â€¢ 6:07:20 PM" \
  --timezone America/New_York
```

## Live Mode

Live mode can change a real account username. It requires all three:

1. A valid configured bearer token.
2. The `--live` flag.
3. The exact acknowledgement environment variable.

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

Easy mode handles the acknowledgement internally for the single guided run after asking for confirmation.

## Accounts

Use `tokens.txt` for one or more accounts:

```text
first-account-token
second-account-token
third-account-token
```

Check all configured accounts:

```bash
python Main.py check-accounts
```

## Proxies

Easy mode intentionally uses a direct connection. Advanced commands can use proxies when `proxy.enabled: true` and HTTP(S) proxies are configured.

```bash
python Main.py check-proxies
python Main.py test-proxies
```

## Discord

Discord is webhook-only and optional. It runs outside the precision timer and is disabled during safe dry runs.

```bash
python Main.py config-validate
```

## Good First Workflow

```bash
python Main.py config-create
python Main.py simulate
python Main.py easy
python Main.py config-validate
```

Then, before any real claim:

```bash
python Main.py check-accounts
python Main.py benchmark
```

## Common Mistakes

**Using a past example date**

The example date `September 15, 2026` is only an example. Use a future drop time copied from NameMC.

**Expecting dry runs to contact Discord or Minecraft**

Dry runs are intentionally local and safe.

**Putting secrets in screenshots or issues**

Never share bearer tokens, webhook URLs, proxy credentials, or full config files.

**Using SOCKS proxies**

Current proxy handling supports HTTP(S), not SOCKS.
