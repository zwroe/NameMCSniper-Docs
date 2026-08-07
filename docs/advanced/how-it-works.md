# How It Works

NameMCSniper v3.0.0 is built around a safety-first flow: simulations are local by default, and live claims require explicit unlocking.

## Safety Model

By default, commands run in dry-run mode. In dry-run mode the program does not contact:

- Minecraft claim APIs
- Discord webhooks
- Proxies
- NTP servers
- Public HTTPS time APIs

Live claims require:

1. `snipe.dry_run: false` for the runtime config.
2. The `--live` command flag.
3. `NAMEMC_SNIPER_LIVE_ACK=I_UNDERSTAND_THIS_CHANGES_A_REAL_ACCOUNT`.

Without the live flag and acknowledgement, non-local claim APIs are rejected before an HTTP session is created.

## Main Phases

### 1. Configuration Load

The app loads `config.yaml`, then reads sibling files:

- `tokens.txt` for bearer tokens
- `proxies.txt` for HTTP(S) proxies when proxy mode is enabled

It merges values, removes duplicates, validates schema keys, and rejects obsolete or misspelled config options.

### 2. Validation

Validation depends on mode:

- Dry runs can run without tokens.
- Live claims require at least one valid-looking token.
- Scheduled live claims require an explicit timezone.
- Discord live notifications require a valid HTTPS Discord webhook URL.
- Proxy mode requires at least one configured proxy.

### 3. Time Parsing

Scheduled commands accept copied NameMC drop windows:

```text
9/15/2026 â€¢ 6:06:50 PM
```

The parser supports:

- Regular colons
- The older mathematical-colon variant
- IANA timezones such as `America/New_York`
- `UTC`
- Offsets such as `-04:00`

Ambiguous or nonexistent daylight-saving times are rejected when an IANA timezone is used.

### 4. Long Wait

For scheduled runs, the app calculates the claim start time from:

```text
drop_time - start_sniping_at_seconds
```

Long waits show progress:

- Every five minutes during long waits
- Every minute during the final hour
- Every second during the final minute

Clock offset is refreshed every 30 minutes while waiting. If external clock sources are unavailable, the run continues using the local system clock.

### 5. Final Preparation

Live-only preparation is deferred until shortly before the claim window:

- Token validation
- Final clock sync
- Connection pre-warming
- Process-priority setup
- Garbage-collector changes

Process-priority failures are warnings, not fatal errors.

### 6. Claim Window

Workers run concurrently until one of these happens:

- A claim succeeds
- The global attempt budget is exhausted
- The claim window ends
- The run is interrupted

`max_snipe_attempts` is a global budget shared by all workers, not a per-worker limit.

### 7. Results and Cleanup

The app reports a mode-aware result:

- Simulation messages cannot be confused with real claims.
- Live success messages only appear after a real accepted response.
- HTTP sessions and other resources are cleaned up after each run.

## Accounts

All configured tokens are preflighted. Invalid or ineligible tokens are disabled for the current claim window. Multiple valid tokens are distributed across workers.

!!! warning "Token Lifetime"
    Bearer tokens are not automatically refreshed. Tokens must still be valid when the live claim window begins.

## Proxies

Proxies are optional and only support HTTP(S). The proxy manager tracks health, disables failing proxies, rotates healthy proxies, and redacts proxy credentials in logs.

Easy mode disables proxies for the run.

## Discord

Discord notifications use webhooks only. They run outside the precision timer with bounded timeouts. Rate-limited notifications are skipped rather than blocking the claim path.

Easy mode disables Discord for the run.

## Local Integration Testing

The test suite uses a loopback fake Minecraft API to exercise request serialization, headers, response handling, connection pooling, and cleanup without reaching Minecraft.
