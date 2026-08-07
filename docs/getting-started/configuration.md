# Configuration

NameMCSniper v3.0.0 uses `config.yaml` for normal settings and optional sibling files for sensitive lists.

## Files

| File | Purpose | Commit? |
|------|---------|---------|
| `config.yaml` | Local runtime configuration | No |
| `tokens.txt` | One Minecraft bearer token per line | No |
| `proxies.txt` | One HTTP(S) proxy per line | No |
| `config.yaml.example` | Shareable example config | Yes |

Create the default local config:

```bash
python Main.py config-create
```

## Current Config Shape

```yaml
snipe:
  target_username: "TestName"
  bearer_token: ""
  bearer_tokens: []
  timezone_name: "America/New_York"
  dry_run: true
  dry_run_scenario: "taken_then_success"
  dry_run_success_after: 3
  api_base_url: "https://api.minecraftservices.com"
  start_sniping_at_seconds: 0.4
  snipe_window_seconds: 10.1
  max_snipe_attempts: 3000
  request_delay_ms: 8
  concurrent_requests: 40
  max_backoff_seconds: 5
  adaptive_delays: true
  per_token_rate_limiting: true

proxy:
  enabled: false
  proxies: []
  rotation_enabled: true
  timeout: 10
  max_retries: 3

discord:
  enabled: false
  webhook_url: ""
  mention_role_id: ""
  embed_color: 65280

notifications:
  intervals:
    - 3600
    - 1800
    - 300
    - 60
    - 30
    - 10
    - 5
    - 1

performance:
  gc_disable: true
  high_priority: true
  pre_warm_connections: true
  busy_wait_ms: 50

debug_mode: false
log_level: "INFO"
```

## Snipe Settings

| Setting | Description |
|---------|-------------|
| `target_username` | Username used by default commands and easy mode |
| `bearer_token` | Primary token, usually kept empty if using `tokens.txt` |
| `bearer_tokens` | Additional YAML tokens; `tokens.txt` is preferred for secrets |
| `timezone_name` | Timezone for copied NameMC timestamps, such as `America/New_York`, `UTC`, or `-04:00` |
| `dry_run` | Safe mode switch; defaults to `true` |
| `dry_run_scenario` | Simulation outcome: `success`, `taken`, `taken_then_success`, `rate_limited_then_success`, `auth_error`, `server_error`, or `timeout` |
| `start_sniping_at_seconds` | Lead time before the drop window starts |
| `snipe_window_seconds` | Claim attempt window length |
| `max_snipe_attempts` | Global attempt budget shared by all workers |
| `request_delay_ms` | Per-worker pacing delay |
| `concurrent_requests` | Number of concurrent workers |
| `adaptive_delays` | Back off based on rate-limit responses |
| `per_token_rate_limiting` | Track rate limits separately per token |

## Tokens File

Create `tokens.txt` beside `config.yaml`:

```text
# One token per line. Blank lines and comments are ignored.
first-account-token
second-account-token
third-account-token
```

Tokens from `tokens.txt` are merged with YAML tokens, duplicates are removed, and file-only tokens are not copied into YAML when configuration is saved.

## Proxy Settings

| Setting | Description |
|---------|-------------|
| `enabled` | Enables proxy use for advanced commands |
| `proxies` | YAML proxy list; `proxies.txt` is preferred |
| `rotation_enabled` | Enables round-robin proxy selection |
| `timeout` | Proxy request timeout in seconds |
| `max_retries` | Failures before a proxy is disabled |

Supported proxy formats:

```text
http://host:port
http://username:password@host:port
https://host:port
https://username:password@host:port
```

SOCKS proxies are rejected by the current aiohttp transport.

## Discord Settings

Discord support is webhook-only.

| Setting | Description |
|---------|-------------|
| `enabled` | Enables webhook notifications for non-dry-run flows |
| `webhook_url` | HTTPS Discord webhook URL |
| `mention_role_id` | Optional numeric role ID |
| `embed_color` | Decimal embed color |

Easy mode disables Discord and proxies intentionally so they stay out of the critical path.

## Validate Configuration

```bash
python Main.py config-validate
```

Safe dry runs can validate without a token. Live scheduled claims require at least one valid-looking token and an explicit timezone.
