# Sniping tips

## Choose a time you can verify

Start with **1 · Schedule a name change**. For a three-character name, the menu shows any future bounds from 3NAME. You can inspect the same estimate with `namemcsniper drop NAME`. Look at both timestamps and how far apart they are. The name may still be unavailable at the earliest bound, and another player may claim it before the latest bound. If the bounds are hours apart, check for an updated estimate closer to the drop or use a time you have independently verified. [3NAME](https://3name.xyz/) · [MCsniperGO's notes on drop windows](https://github.com/Kqzz/MCsniperGO#claim-range)

The automatic command-line schedule accepts a 3NAME window up to 30 seconds wide. The same limit applies to a manual range made with `--at` and `--until`. A single attempt over an accepted range is scheduled for its latest bound. In the menu, you can select an individual future bound or enter a time with an explicit UTC offset. Review the UTC time in the preview before continuing. [Schedule options](../usage/basic-usage.md)

## Check the clock and account early

1. On Windows, open **Settings > Time & language > Date & time** and turn on **Set time automatically**. Choose **6 · Check UTC clock** in NameMCSniper and check that the sources agree. Read the estimated UTC uncertainty alongside the offset. [Microsoft's Windows time settings guide](https://support.microsoft.com/en-us/windows/experience/personalization/set-time-date-and-time-zone-settings-in-windows)
2. Use **2 · Check my Minecraft account** or **7 · Test bearer tokens (tokens.txt)** to confirm the Java profile UUID and rename eligibility. For a future run, NameMCSniper checks eligibility again shortly before dispatch. Minecraft allows one Java profile name change every 30 days. [Authentication](../getting-started/authentication.md) · [Minecraft Help](https://help.minecraft.net/hc/en-us/articles/4408950195341-View-or-Change-Your-In-Game-Profile-Name-in-Minecraft)
3. Use a stable connection and keep the machine awake until the outcome appears. If the clock check fails, resolve it while there is still time to preview the schedule again. [Clock troubleshooting](troubleshooting.md#clock-checks)

## Read the timing numbers

Choose **5 · Measure dispatch timing** or run `namemcsniper benchmark --samples 100`. It runs the scheduler offline against scripted responses and reports the median, 95th percentile, and worst dispatch delay, plus missed slots. Run it on the machine you plan to use under its usual load.

| Measurement | What it measures |
| --- | --- |
| Benchmark dispatch delay | How late the local scheduler starts a scripted request relative to its slot. |
| UTC offset and estimated uncertainty | How far the system clock appears to be from UTC and how uncertain the app's correction is. |
| Clock check round trip | The round trip to a public NTP server. The elapsed time reported for a live attempt covers work with Minecraft Services, including any follow-up profile check. |

An offline benchmark cannot tell you when Minecraft will process a request or when the name will actually drop. Start with the default single attempt and stop if Minecraft returns HTTP 429. Follow `Retry-After` when the response provides it. MCsniperGO's maintainers reported that rate limits made automated claims ineffective in their testing. [MCsniperGO maintainer notice](https://github.com/Kqzz/MCsniperGO#readme)
