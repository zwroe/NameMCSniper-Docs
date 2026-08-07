# Performance Optimization

Performance tuning should start with measurement, not maximum settings. In v3.0.0, safe simulation and benchmarks let you test locally before any live run.

## Start With These Checks

```bash
python Main.py simulate
python Main.py benchmark
python Main.py config-validate
python Main.py check-accounts
```

If proxy mode is enabled:

```bash
python Main.py check-proxies
```

## Important Settings

```yaml
snipe:
  start_sniping_at_seconds: 0.4
  snipe_window_seconds: 10.1
  max_snipe_attempts: 3000
  request_delay_ms: 8
  concurrent_requests: 40
  max_backoff_seconds: 5
  adaptive_delays: true
  per_token_rate_limiting: true

performance:
  gc_disable: true
  high_priority: true
  pre_warm_connections: true
  busy_wait_ms: 50
```

## Concurrency

`concurrent_requests` controls worker count. More workers are not always better:

- They share one global attempt budget.
- Too many workers can trigger rate limits.
- More valid tokens usually helps more than forcing one token harder.
- Bad proxies can erase any concurrency advantage.

Increase concurrency gradually and compare results with `simulate`, `benchmark`, and live-safe checks.

## Request Delay

`request_delay_ms` controls per-worker pacing. Low values are more aggressive. Keep `adaptive_delays` enabled so rate-limit responses can slow workers when needed.

## Attempt Budget

`max_snipe_attempts` is global across all workers and tokens. For example, 40 workers do not each get 3000 attempts; they share the budget.

## Clock Behavior

Scheduled runs refresh clock offset every 30 minutes while waiting. If NTP and HTTPS time fallbacks fail, NameMCSniper continues with the local system clock.

For live timing:

- Keep the machine clock correct.
- Avoid suspending the computer during long waits.
- Prefer stable machines for long schedules.
- Run `benchmark` on the machine you plan to use.

## Long Schedules

For drops hours or days away:

- Progress logs appear every five minutes.
- The final hour logs every minute.
- The final minute logs every second.
- Token checks and connection preparation happen near the claim window, not hours early.

## Proxies

Only use proxies that are stable and low latency.

```bash
python Main.py check-proxies --concurrency 50
```

Good proxy practice:

- Use HTTP(S), not SOCKS.
- Remove slow or dead proxies.
- Do not assume proxies beat a clean direct VPS route.
- Keep credentials private.

## Discord

Discord webhooks are intentionally outside the precision timer. Rate limits or webhook failures should not block the critical claim path.

## VPS Notes

A VPS is not required for correctness testing. It can help with uptime, routing, and stable clocks, but you should still measure:

```bash
python Main.py benchmark
python Main.py simulate
python Main.py check-accounts
```

## Risk Balance

Aggressive settings can increase rate limits or failures. Tune in small steps and keep a record of:

- Worker count
- Delay
- Attempt budget
- Proxy use
- Token count
- Benchmark jitter
- Network route
