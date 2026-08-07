# Proxy Setup

NameMCSniper v3.0.0 supports HTTP(S) proxies for advanced commands. Easy mode intentionally uses a direct connection.

## When to Use Proxies

Use proxies only after testing direct latency and stability. Poor proxies can be slower than a clean direct VPS connection and can increase failure rates.

## Supported Formats

```text
http://host:port
http://username:password@host:port
https://host:port
https://username:password@host:port
```

SOCKS proxies are not supported by the current aiohttp transport and are rejected by validation.

## Recommended Storage

Set proxy mode in `config.yaml`:

```yaml
proxy:
  enabled: true
  proxies: []
  rotation_enabled: true
  timeout: 10
  max_retries: 3
```

Then create `proxies.txt` beside `config.yaml`:

```text
# One HTTP(S) proxy per line. Blank lines and comments are ignored.
http://username:password@proxy1.example.com:8080
http://username:password@proxy2.example.com:8080
```

File proxies are merged with YAML proxies, duplicates are removed, and file-only proxies are not copied into YAML when the app saves config.

## Validate Proxies

```bash
python Main.py check-proxies
```

Useful options:

```bash
python Main.py check-proxies --concurrency 100 --output proxies_working.txt
```

`check-proxies` tests configured proxies against the same service family used for claims and saves working proxies to the output file.

You can also run:

```bash
python Main.py test-proxies
```

## Rotation and Health

When `rotation_enabled` is true, the proxy manager chooses healthy proxies in round-robin order.

Proxy health tracking includes:

- Success count
- Failure count
- Consecutive failures
- Response time
- Automatic disabling after repeated failures
- Limited recovery checks when no healthy proxy remains

Proxy credentials are redacted in logs.

## Tuning

| Setting | Guidance |
|---------|----------|
| `timeout` | Start around `10`; lower only after stable tests |
| `max_retries` | Controls failures before a proxy is disabled |
| `rotation_enabled` | Keep enabled for multiple proxies |
| `concurrent_requests` | Do not raise just because proxies exist |

## Common Problems

**SOCKS proxy rejected**

Use HTTP or HTTPS proxies.

**Proxy list empty but proxy support enabled**

Add proxies to `proxies.txt` or the YAML `proxy.proxies` list.

**Proxy authentication failed**

Check username, password, host, and port. URL-encode special characters in credentials if needed.

**Direct connection is faster**

Use direct mode. Proxies are optional and can hurt timing.

## Security

- Do not commit `proxies.txt`.
- Do not post proxy URLs with credentials.
- Use reputable providers.
- Remove dead proxies regularly.
