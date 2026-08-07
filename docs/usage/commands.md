# Command Reference

Current command reference for NameMCSniper v3.0.0.

```bash
python Main.py [OPTIONS] COMMAND [ARGS]...
```

Global options:

| Option | Description |
|--------|-------------|
| `--version` | Show the CLI version |
| `--help` | Show help |

## Commands

| Command | Purpose |
|---------|---------|
| `benchmark` | Run a timing benchmark to test system precision |
| `check-accounts` | Validate all configured bearer tokens |
| `check-proxies` | Check and validate configured proxies |
| `config-create` | Create a default configuration file |
| `config-validate` | Validate configuration file |
| `easy` | Run a beginner-friendly guided simulation or real claim |
| `simulate` | Run the complete claim worker path without external network access |
| `snipe` | Run an immediate dry-run claim unless `--live` is unlocked |
| `snipe-at` | Run a scheduled dry-run claim unless `--live` is unlocked |
| `snipe-fallback` | Run dry-run fallback windows unless `--live` is unlocked |
| `test-proxies` | Test proxy connections through the proxy manager |
| `test-token` | Test the primary configured bearer token |
| `version` | Show version information |

## `easy`

```bash
python Main.py easy [--config config.yaml]
```

Guided flow for simulations and live runs. Easy mode disables proxies and Discord for the run, uses a direct connection, and asks before any live claim.

## `simulate`

```bash
python Main.py simulate [--config config.yaml] [--username TestName] [--scenario taken_then_success] [--success-after 3]
```

Runs the worker path without external network access.

Scenarios:

- `success`
- `taken`
- `taken_then_success`
- `rate_limited_then_success`
- `auth_error`
- `server_error`
- `timeout`

Examples:

```bash
python Main.py simulate
python Main.py simulate --username "TargetName" --scenario success
python Main.py simulate --scenario rate_limited_then_success --success-after 4
```

## `snipe`

```bash
python Main.py snipe [--config config.yaml] [--username TargetName] [--token TOKEN] [--live]
```

Runs an immediate claim attempt. Without `--live`, this is a dry run.

Live mode also requires:

```text
NAMEMC_SNIPER_LIVE_ACK=I_UNDERSTAND_THIS_CHANGES_A_REAL_ACCOUNT
```

## `snipe-at`

```bash
python Main.py snipe-at -u "TargetName" -w "9/15/2026 â€¢ 6:06:50 PM" --timezone America/New_York
```

Options:

| Option | Required | Description |
|--------|----------|-------------|
| `--username`, `-u` | Yes | Username to claim |
| `--drop-window`, `-w` | Yes | NameMC time string |
| `--timezone` | For live scheduled claims | IANA timezone, `UTC`, or offset such as `-04:00` |
| `--config`, `-c` | No | Config file path |
| `--live` | No | Unlock real claim mode when acknowledgement is also set |

## `snipe-fallback`

```bash
python Main.py snipe-fallback \
  -u "TargetName" \
  -t "9/15/2026 â€¢ 6:06:50 PM" \
  -t "9/15/2026 â€¢ 6:07:20 PM" \
  --timezone America/New_York
```

Requires at least two `--times` / `-t` values.

## `config-create`

```bash
python Main.py config-create [--config config.yaml]
```

Creates a default local configuration file.

## `config-validate`

```bash
python Main.py config-validate [--config config.yaml]
```

Validates config schema, live-mode requirements, webhook URL shape, proxy list presence, and timezone requirements.

## `test-token`

```bash
python Main.py test-token [--config config.yaml]
```

Checks the primary configured bearer token against the Minecraft profile API.

## `check-accounts`

```bash
python Main.py check-accounts [--config config.yaml]
```

Validates all configured tokens from YAML and `tokens.txt`.

## `check-proxies`

```bash
python Main.py check-proxies [--config config.yaml] [--output proxies_working.txt] [--concurrency 50]
```

Checks configured HTTP(S) proxies and saves working proxies to the output file.

## `test-proxies`

```bash
python Main.py test-proxies [--config config.yaml]
```

Tests proxy connections and prints summary stats.

## `benchmark`

```bash
python Main.py benchmark [--requests 10] [--config config.yaml]
```

Measures local timing precision and reports offset and jitter.

## `version`

```bash
python Main.py version
```

Shows:

```text
NameMCSniper v3.0.0
Safe-by-default username claim simulator and client
```

## Removed or Old Commands

Older documentation referenced menu mode, `generate-token`, `check-username`, `--dry-run`, `--debug`, and Discord bot settings. Those are not part of the current v3.0.0 command surface documented here.
