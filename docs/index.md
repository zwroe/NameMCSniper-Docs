# NameMCSniper

![NameMCSniper banner](namemcsniperbanner.png)

NameMCSniper schedules Minecraft Java Edition profile name changes. The Rust build opens a guided terminal menu for checking accounts, previewing a schedule, and confirming a live rename.

## Start here

1. Open the Rust executable. On Windows, double-click `namemcsniper.exe`. [Installation](getting-started/installation.md)
2. Choose **3 · Try an offline simulation** to see how a result looks.
3. Choose **2 · Check my Minecraft account** for one bearer token, or **7 · Test bearer tokens (tokens.txt)** for a file. [Authentication](getting-started/authentication.md)
4. Choose **1 · Schedule a name change**. Enter the target, pick a time, and review the account and schedule before you confirm. [Schedule a rename](usage/basic-usage.md)

For three-character names, the menu can pull the estimated release window from 3NAME. You can also enter a specific time. [Sniping tips](advanced/performance.md)

## Why I switched from Python to Rust

I wanted NameMCSniper to open from Explorer and give me more control over when a rename request leaves the app. The Rust build produces a native executable. I moved request preparation ahead of the drop and used a monotonic timer for the final wait. The app rechecks UTC if it detects a change to the system clock.

The rewrite also checks rename eligibility before dispatch, keeps a verified UTC correction through brief outages, and reports results for each account in a group. The offline benchmark measures local dispatch delay. The drop estimate, network path, and Minecraft's response determine what happens after that. [How it works](advanced/how-it-works.md)

## Using these docs

These pages cover the Rust checkout and its builds. The public GitHub release still contains the Python edition. [Python v3.0.0 release](https://github.com/zwroe/NameMCSniper/releases/tag/v3.0.0)

- [Local files and settings](getting-started/configuration.md) covers `tokens.txt`, `proxies.txt`, and run logs.
- [Command reference](usage/commands.md) covers the terminal commands and flags.
- [Troubleshooting](advanced/troubleshooting.md) covers missed times, token errors, and rate limits.
