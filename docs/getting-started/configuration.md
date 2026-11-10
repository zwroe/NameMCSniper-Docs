# Local files and settings

Set the target, timing, and account from the guided menu or with [command-line flags](../usage/commands.md). Keep optional token and proxy lists in plain UTF-8 text files.

| File | Contents | Menu action |
|---|---|---|
| `tokens.txt` | One Minecraft Services bearer token per line | **7 · Test bearer tokens (tokens.txt)**, or **Use all eligible profiles from tokens.txt** during live setup |
| `proxies.txt` | One HTTP or HTTPS proxy per line | **4 · Test proxies** |
| `proxies_working.txt` | Passing entries from a proxy test, including any credentials | Optional export from **4 · Test proxies** |

## Where to put the lists

Create `tokens.txt` or `proxies.txt` in the folder with `namemcsniper.exe` if you need one. The menu shows the default path and accepts another path. It looks in the working directory first, then beside the executable. When you omit the file argument, `check-tokens` uses the same search order and `test-proxies` looks only in the working directory.

Both input lists accept blank lines and comments that start with `#`. Keep lists containing credentials out of version control and screenshots. See [Authentication](authentication.md) and [Proxy setup](../usage/proxy-setup.md) for formats and limits.

## Set the time

Use a 3NAME estimate for a three-character name, pick **Two minutes from now** in the guided menu, or enter an absolute time. Absolute inputs accept RFC 3339 with a time zone offset, such as `2030-06-01T14:00:00.250-04:00`, or Unix milliseconds. Replace the example with your chosen time. [Scheduling a rename](../usage/basic-usage.md)
