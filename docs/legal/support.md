# Support

Start with the current docs and troubleshooting guide before opening an issue.

## Self-Checks

```bash
python Main.py --version
python Main.py config-validate
python Main.py simulate
python Main.py benchmark
```

For accounts:

```bash
python Main.py check-accounts
```

For proxies:

```bash
python Main.py check-proxies
```

## Primary Support

Use GitHub Issues for reproducible bugs:

[https://github.com/zwroee/NameMCSniper/issues](https://github.com/zwroee/NameMCSniper/issues)

Before opening an issue:

- Search existing issues.
- Confirm you are on v3.0.0 or the latest `main`.
- Reproduce with a safe simulation when possible.
- Remove secrets from logs and config.

## What to Include

```text
NameMCSniper version:
Python version:
Operating system:
Command:
Expected behavior:
Actual behavior:
```

Include relevant logs only after redacting:

- Bearer tokens
- Discord webhook URLs
- Proxy usernames/passwords
- Private account details

## Discord Community

The docs config currently links this Discord invite:

[https://discord.gg/b4BFqqtTqF](https://discord.gg/b4BFqqtTqF)

Use community support for setup questions, not for sharing secrets.

## Security Issues

Do not open public issues for token leaks, credential exposure, or exploitable vulnerabilities. Contact the maintainers privately through the repository owner profile or an agreed private channel.

## Support Limits

Support cannot guarantee:

- Username claim success
- Token refresh
- Proxy quality
- VPS network routing
- Recovery from account or service restrictions

Use live functionality only where permitted.
