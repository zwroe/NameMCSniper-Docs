# Troubleshooting

## Launching the app

On Windows, open `namemcsniper.exe` to start the menu. If PowerShell says the command is unknown, open a terminal in the folder that contains the executable and run `.\namemcsniper.exe`. You can also use its full path. [Installation](../getting-started/installation.md)

## The token or proxy list is missing

If a list is missing, the menu shows the expected path. Copy `tokens.txt.example` or `proxies.txt.example` into the displayed folder, remove `.example` from the copy’s name, and add one entry per line. If you create the file in Windows Notepad, choose **Save as type > All files** to avoid a filename such as `tokens.txt.txt`. Keep credential files private.

Menu options **7 · Test bearer tokens (tokens.txt)** and **4 · Test proxies** show the default path before reading it. They look in the working folder first and then beside the executable. Press Enter to use the displayed path, or paste the full path to another file. The `check-tokens` command uses the same search order when you omit the path. The `test-proxies` command defaults to the working folder. Pass a file path to use a different list. [Token file format](../getting-started/authentication.md) · [Proxy file format](../usage/proxy-setup.md)

```powershell
.\namemcsniper.exe check-tokens 'C:\Path To Files\tokens.txt'
.\namemcsniper.exe test-proxies 'C:\Path To Files\proxies.txt'
```

## Account checks

- **Invalid or expired token.** Get a fresh Minecraft Services bearer token from your own signed-in Minecraft session, then use **2 · Check my Minecraft account** to verify it. Use the hidden prompt for a single token. [Authentication](../getting-started/authentication.md)
- **Java profile missing or UUID mismatch.** Check that the signed-in account has an existing Java Edition profile. Use the Java profile UUID returned by the checker when specifying `--account`. [Minecraft Help](https://help.minecraft.net/hc/en-us/articles/4408950195341-View-or-Change-Your-In-Game-Profile-Name-in-Minecraft)
- **Profile cannot change its name yet.** Wait until Minecraft reports it can rename. The Java Edition rename cooldown is 30 days. A token can be valid while the profile is on cooldown. [Minecraft Help](https://help.minecraft.net/hc/en-us/articles/4408950195341-View-or-Change-Your-In-Game-Profile-Name-in-Minecraft)

## Drop estimates and times

- **3NAME gives no usable estimate.** The automatic lookup covers three-character names. If the estimate is missing, expired, or too wide for automatic scheduling, use a future time you can verify. [3NAME](https://3name.xyz/)
- **The timestamp is rejected.** Include `Z` or a numeric offset, as in `2030-06-01T18:00:00.250Z` or `2030-06-01T14:00:00.250-04:00`. The CLI also accepts Unix milliseconds. An automatic 3NAME window or an explicit `--at` and `--until` range must fit within 30 seconds. [Schedule options](../usage/basic-usage.md)
- **The schedule ends without sending.** Check the log for a skipped slot or a failed account check. If the machine was asleep or busy when the slot arrived, the one-second grace period may have elapsed. Review the time and choose a valid future slot.

## Clock checks

- **Could not get an agreed UTC offset.** Choose **6 · Check UTC clock** again on a stable connection. The app needs replies from two public NTP sources that agree within 50 ms. If the failure persists, check whether the network or firewall permits outbound UDP port 123. If it is a managed network, use a network that allows NTP. [NTP specification](https://www.rfc-editor.org/rfc/rfc5905.html)
- **Offset exceeds five minutes.** On Windows, open **Settings > Time & language > Date & time** and enable **Set time automatically**. Use **Sync now** if available, then run the app's check again. Verify the system date and time if the offset persists. [Microsoft's Windows time settings guide](https://support.microsoft.com/en-us/windows/experience/personalization/set-time-date-and-time-zone-settings-in-windows)
- **Estimated uncertainty is too high or the clock changed.** Check the network and keep the computer awake. Let the system time settle after a manual change, then run **6 · Check UTC clock**. An absolute live run stops if it cannot verify UTC. Restart with a verified clock and a future time. [How clock verification works](how-it-works.md#verify-utc-and-wait)

## Proxy tests

Check that each entry uses a supported HTTP or HTTPS proxy format. `healthy (HTTP 401)` means the unauthenticated HTTPS probe reached Minecraft Services and received the expected response. If exporting fails, use a new filename because the checker requires a new output file. Exported entries retain credentials. [Proxy setup](../usage/proxy-setup.md)

## Live outcomes

- **HTTP 429 or exit code `3`.** Minecraft Services rate limited an account check or rename request. A rate limit during preflight stops the run before dispatch. If the app displays `Retry-After`, wait at least that long before trying again.
- **Outcome uncertain or exit code `4`.** Run `namemcsniper check` and look at the current profile name. For a group run, use the outcome shown for each profile to decide which accounts to check. Do the same after cancelling during a request. Minecraft may have processed the rename before the reply was lost.
- **Name still unavailable.** Compare the dispatched UTC time with the drop estimate. The estimate can differ from the actual release, and another player may have claimed the name.

## Logs

Each live run prints its log path. On Windows, open `%LOCALAPPDATA%\NameMCSniper\logs` in File Explorer. On macOS, look under `~/Library/Logs/NameMCSniper`. On Linux, look under `$XDG_STATE_HOME/namemcsniper/logs` or `~/.local/state/namemcsniper/logs` when `XDG_STATE_HOME` is unset.

If you ask for help, include the app version from `namemcsniper --version`, your operating system, and the relevant error or log lines. Redact profile details you want to keep private and keep bearer tokens and proxy credentials out of the report. [Support](../legal/support.md)
