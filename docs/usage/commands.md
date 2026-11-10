# Command reference

Launch `namemcsniper` with no arguments in a terminal for the guided menu. The examples use the executable name on your PATH. In PowerShell, use `.\namemcsniper.exe` if you are running it from the current folder. [Installation](../getting-started/installation.md).

| Command | Behavior |
| --- | --- |
| `menu` | Opens the guided menu. |
| `check [--token-stdin]` | Checks one Minecraft Services bearer token, Java profile, and rename eligibility. Prompts for the token if needed. |
| `check-tokens [FILE]` | Checks tokens from a file and reports each result by line. With no path, looks for `tokens.txt` in the current folder first, then beside the executable. |
| `test-proxies [FILE] [--output FILE]` | Tests a proxy list and optionally writes passing entries to a new file. Defaults to `proxies.txt` in the current folder. |
| `drop NAME [--at TIME] [--until TIME]` | Looks up a three-character name on 3NAME or displays the time or range you entered. |
| `snipe NAME [OPTIONS]` | Previews a schedule. To go live, add `--live` and specify a profile or token file. |
| `simulate [--scenario SCENARIO]` | Runs an offline simulation. Scenarios are `success`, `unavailable`, `rate-limited`, `uncertain`, and `rejected`. Defaults to `success`. |
| `benchmark [--samples N]` | Measures local dispatch delay offline. Supports 1 to 200 samples and defaults to 40. |
| `clock-check` | Checks UTC against public time sources. |
| `--help` | Lists commands. Add it after a command to see that command’s options. |
| `--version` | Prints the version of the executable. |

The Rust CLI also recognizes the aliases `check-accounts` for `check`, `test-tokens` for `check-tokens`, and `snipe-at` for `snipe`.

## Look up or preview a time

```text
namemcsniper drop abc
namemcsniper snipe ExampleName
namemcsniper snipe ExampleName --at 2030-06-01T18:00:00.250Z
namemcsniper snipe ExampleName --at 2030-06-01T18:00:00Z --until 2030-06-01T18:00:20Z --attempts 3
```

`drop NAME` looks up 3NAME for names made of exactly three letters, digits, or underscores. A lookup may return a wide range or only a latest time. An offline `snipe NAME` without `--at` previews an example time two seconds in the future. Add `--at` to preview a timestamp you supply.

## Schedule a live change

Run `check` or `check-tokens` first to find your Java UUID and confirm rename eligibility. Replace `UUID`, `FIRST_UUID`, and `SECOND_UUID` in the examples with the UUIDs you select. Use accounts you control. [Authentication](../getting-started/authentication.md).

```text
namemcsniper snipe ExampleName --at 2030-06-01T18:00:00.250Z --live --account UUID
namemcsniper snipe ExampleName --at 2030-06-01T18:00:00.250Z --live --tokens-file tokens.txt
namemcsniper snipe ExampleName --at 2030-06-01T18:00:00.250Z --live --account FIRST_UUID --account SECOND_UUID --tokens-file tokens.txt
```

| Flag | Behavior |
| --- | --- |
| `--at TIME` | Sets the first time of an exact schedule or the earliest time of a bounded schedule. Use RFC 3339 with `Z` or an explicit offset, or Unix milliseconds. |
| `--until TIME` | Sets the latest time of a range. Requires `--at`. For scheduling, the range can be at most 30 seconds wide. |
| `--attempts N` | Defaults to 1. Selecting exactly one profile with `--account` allows up to 3 slots. |
| `--interval SECONDS` | Sets the minimum spacing between slots. Defaults to 10 seconds and supports 10 to 30. |
| `--live` | Enables live requests and requires `--account UUID` or `--tokens-file FILE`. |
| `--account UUID` | Selects a Java profile and requires `--live`. Repeat with `--tokens-file` to select up to 64 distinct profiles. |
| `--tokens-file FILE` | Loads up to 64 token entries and requires `--live`. Without `--account`, selects every distinct eligible profile and skips profiles that already hold the target name. |
| `--token-stdin` | Reads a single bearer token from the first line of standard input. Requires `--live` for `snipe` and is mutually exclusive with `--tokens-file`. |

A single-account run can read the token from a hidden prompt or `NAMEMCSNIPER_TOKEN` when neither token flag is set. A token-file run with no UUIDs or with multiple UUIDs requires the default one slot. The app dispatches one concurrent request per ready profile at that slot.

A live run without `--at` looks up 3NAME for a three-character target and needs a usable window of at most 30 seconds. All absolute live schedules require a verified UTC clock. A bounded schedule with one slot uses the latest time. If you request more slots, the app spaces them within the range and uses fewer when the interval leaves insufficient room. An exact schedule starts at `--at` and spaces later slots by the interval.

With exactly one `--account`, a scheduled retry is possible only after the service confirms the name is unavailable. A rate limit or uncertain result stops further attempts. [Timing and preparation](../advanced/performance.md).

## Exit codes

| Code | Meaning |
| --- | --- |
| `0` | Completed successfully. A group can return success when one profile confirmed a name change. |
| `1` | A command failed. For `check-tokens`, one or more token checks failed. |
| `2` | Invalid command-line arguments. |
| `3` | Minecraft Services rate limited a single-account check, a token-list check, or live preflight. This code also applies when rate limiting is the final rename outcome. |
| `4` | The live name-change outcome was uncertain. Check the run log and profile. |
| `130` | Live run interrupted with Ctrl+C. If a request was in flight, check the selected profiles before trying again. |

The app displays the `Retry-After` wait when Minecraft Services supplies one. A rate limit during preflight stops the run before dispatch.
