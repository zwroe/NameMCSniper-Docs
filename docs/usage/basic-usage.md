# Schedule a rename

Launch the Rust executable with no arguments to open the menu. On Windows, open `namemcsniper.exe` or run it in PowerShell.

```powershell
.\namemcsniper.exe
```

## Practice with the menu

| Menu choice | What it does |
| --- | --- |
| **1. Schedule a name change** | Choose a target and time, then review an offline preview before deciding whether to go live. |
| **2. Check my Minecraft account** | Check one Minecraft Services bearer token and view the Java profile and rename eligibility. |
| **3. Try an offline simulation** | Rehearse a name change without an account or a live request. |
| **4. Test proxies** | Check HTTPS reachability through the entries in your proxy list. |
| **5. Measure dispatch timing** | Measure how close your computer gets to its scheduled dispatch times in an offline test. |
| **6. Check UTC clock** | Check whether the app can verify UTC using public time sources. |
| **7. Test bearer tokens (tokens.txt)** | Check multiple bearer tokens and see which Java profiles are eligible. |
| **0. Exit** | Close the app. |

## Schedule a live name change

1. Choose **Schedule a name change** and enter the target username.
2. Choose **Two minutes from now**, **Enter a time**, or an available **Use 3NAME Latest** or **Use 3NAME Earliest** option. For a manual time, use a timestamp with a UTC offset, such as `2030-06-01T18:00:00.250Z`, or Unix milliseconds.
3. Read the offline preview. Choose **Continue to live setup** when the target and time are correct.
4. Choose **Paste one token** or **Use all eligible profiles from tokens.txt**. For a list, accept the displayed path or enter your own. The app selects every distinct eligible Java profile from up to 64 token entries and skips profiles that already hold the target name. [Create a token list](../getting-started/authentication.md).
5. Review the target, scheduled time, current names, and UUIDs. Type `RENAME` to confirm or press Enter to cancel.

The menu schedules one slot. For a group, the app sends one concurrent name-change request per profile that passes the final check. The app schedules those checks 20 seconds before the slot for one to four profiles, and up to 2 minutes 45 seconds before the slot for 64 profiles. If you finish setup after that point, checks start as soon as possible.

## Pick a time from 3NAME

For a three-character username, the menu looks up the 3NAME window and offers any usable future endpoint of the range. The range is an estimate. **Use 3NAME Latest** schedules one attempt at the later time, and **Use 3NAME Earliest** schedules one at the earlier time. If no suitable lookup time is available, choose **Enter a time** and use a timestamp you have verified.

Run `drop NAME` to inspect the lookup separately. Absolute live schedules, including 3NAME and manual timestamps, require a verified UTC clock. **Two minutes from now** measures elapsed time from when you choose it. [Timing and preparation](../advanced/performance.md).

## Run logs

A live run prints its log path when the file opens. Use the log to check the selected profiles, dispatch timing, and results. The app keeps bearer tokens out of the log.

| Platform | Default log directory |
| --- | --- |
| Windows | `%LOCALAPPDATA%\NameMCSniper\logs` |
| macOS | `~/Library/Logs/NameMCSniper` |
| Linux | `$XDG_STATE_HOME/namemcsniper/logs` or `~/.local/state/namemcsniper/logs` |

If the result is uncertain, check the final log result and the affected Minecraft profile before another live run.

For terminal flags and exit codes, see the [command reference](commands.md).
