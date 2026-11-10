# Support

Use [Troubleshooting](../advanced/troubleshooting.md) to narrow down an account, timing, or connection problem.

## Reproduce the issue

Run these from a terminal where `namemcsniper` is available. On Windows, you can also open the executable and use the corresponding menu options.

```sh
namemcsniper --version
namemcsniper simulate
namemcsniper benchmark --samples 100
```

For an account problem, use `namemcsniper check` or `namemcsniper check-tokens FILE`. For a timing problem, use `namemcsniper clock-check`. [Command reference](../usage/commands.md)

## Report a bug

Search for an existing report before opening a new one. Include the app version, operating system, the menu steps or command, and what happened. If you built from source, include the revision. [NameMCSniper issues](https://github.com/zwroe/NameMCSniper/issues)

Use the log path printed by a live run to find the relevant excerpt. Redact bearer tokens, proxy credentials, and any private profile details before sharing output or screenshots. Report credential leaks and vulnerabilities privately to the maintainers. [Repository owner](https://github.com/zwroe)
