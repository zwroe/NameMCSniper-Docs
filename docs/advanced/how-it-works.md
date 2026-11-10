# How It Works

NameMCSniper schedules a rename on an existing Minecraft Java Edition profile. The menu lets you review a schedule before choosing an account. A guided live run requires you to type `RENAME` after reviewing the name, time, and selected profiles. From a shell, `snipe --live` requires a profile UUID or a token file. `simulate` and `benchmark` use scripted responses offline. [Command reference](../usage/commands.md)

## Resolve the scheduled time

For a three-character username, NameMCSniper can look up the bounds published by 3NAME. A “Dropping Now” listing can have only a latest bound. The menu offers each future bound as a separate choice. Manual times use RFC 3339 with an explicit offset or Unix milliseconds. The two-minute shortcut counts elapsed time from when you select it. Its displayed UTC time uses the system clock at selection. [3NAME](https://3name.xyz/) · [Schedule options](../usage/basic-usage.md)

## Verify UTC and wait

For an absolute live schedule, NameMCSniper queries Cloudflare, Windows Time, and NIST in parallel using NTP. It requires at least two accepted sources whose offsets agree within 50 ms. The app applies the correction to its own UTC estimate and uses a monotonic clock to track elapsed time toward the scheduled slot. Corrections are capped at five minutes. [NTP offset and filtering](https://www.rfc-editor.org/rfc/rfc5905.html#section-8)

During a long wait before the account check, the app refreshes the correction roughly every two minutes. It also checks again if it detects a clock jump or the countdown wakes unexpectedly late. After a transient refresh failure, it can continue with the last verified correction while the oldest selected sample is under ten minutes old and estimated uncertainty stays at or below 350 ms. If UTC verification fails outside those limits, the absolute schedule stops. [Clock troubleshooting](troubleshooting.md#clock-checks)

## Verify the profiles and prepare the request

The app checks that each token belongs to the selected Java profile UUID and that the profile can rename. It schedules these checks 20 seconds before the first slot for one to four profiles and starts earlier for larger groups, up to 2 minutes 45 seconds before the slot for 64 profiles. The budget covers two phases of up to four concurrent reads, a five-second timeout per read, and five seconds of margin. If less time remains, checks start as soon as possible.

The app prepares the rename request ahead of dispatch and reuses the HTTP client from the account checks. In group mode, it can skip an ineligible profile if another remains. Preflight aborts on HTTP 429 or a profile UUID mismatch. [Authentication](../getting-started/authentication.md)

## Dispatch the rename

The only write is `PUT /minecraft/profile/name/{name}` to Minecraft Services. The guided menu schedules one slot. A script with exactly one `--account` UUID can schedule up to three slots, at least 10 seconds apart. Only a confirmed unavailable result advances to a later slot. Each slot has one second of scheduling grace before the app skips it. A group run uses one slot and sends one concurrent request per ready profile. The token file can contain up to 64 entries. [MCsniperGO's rename request](https://github.com/Kqzz/MCsniperGO/blob/abba9c2f3c16cfdae9c0ee83d0dff3c87769239a/pkg/mc/account.go#L269) · [Command reference](../usage/commands.md)

NameMCSniper confirms a rename when the response identifies both the target name and the authorized profile UUID. If a reply leaves the outcome unclear, the app can read the profile back once to verify the result. HTTP 429 ends the schedule after any requests already in progress, and a rate-limited batch skips follow-up profile reads. If the outcome is still uncertain or you cancel during the request, check the selected profile before sending another rename. [Live outcome troubleshooting](troubleshooting.md#live-outcomes)

## Inspect the results

The terminal displays the outcome for each attempt and, in a group run, for each profile. The live run prints its log path. The log records the schedule, clock checks, dispatch times, and results. It excludes bearer tokens and proxy credentials. The default directory on Windows is `%LOCALAPPDATA%\NameMCSniper\logs`. [Log locations](troubleshooting.md#logs)

The proxy checker makes a separate, unauthenticated HTTPS request through each proxy. Live renames use a direct connection to Minecraft Services. [Proxy setup](../usage/proxy-setup.md)
