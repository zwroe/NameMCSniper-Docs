# Authentication

Account checks and live renames use a Minecraft Services bearer token for an existing Java Edition profile. Treat the token like a password and keep it in a private file, a secret manager, or the app's hidden prompt.

## Find a token from your own session

1. Sign in to [Minecraft My Games](https://www.minecraft.net/en-us/msaprofile/mygames) and open your browser's developer tools to the **Network** panel.
2. Under **Minecraft: Java Edition**, select **Change Profile Name** to open the form. Leave the form open without submitting a rename.
3. Find the `GET /minecraft/profile/namechange` request to `api.minecraftservices.com`. Under **Request Headers**, look for `Authorization`. If it contains `Bearer …`, copy the token after `Bearer `.
4. In NameMCSniper, choose **2 · Check my Minecraft account** and paste at the hidden prompt. Review the Java profile name, UUID, and rename eligibility.

Minecraft allows one Java profile name change every 30 days. A token can pass the account check while the profile is still on cooldown. [Minecraft Help](https://help.minecraft.net/hc/en-us/articles/4408950195341-View-or-Change-Your-In-Game-Profile-Name-in-Minecraft)

## Check a list

Create a private `tokens.txt` beside the executable, with one token per line and an optional `Bearer ` prefix. The checker accepts up to 64 entries, ignoring blank lines and lines beginning with `#`.

Choose **7 · Test bearer tokens (tokens.txt)** and accept the displayed file path or supply your own. The report shows line numbers and profile eligibility. For a shell, use `namemcsniper check-tokens FILE`. Omitting `FILE` checks for `tokens.txt` in the working directory first, then beside the executable.

## Use a token for a live rename

In the menu, paste one token or choose **Use all eligible profiles from tokens.txt**. Review the target, time, and profiles before typing `RENAME`. In scripts, use `--live --tokens-file FILE` to select every distinct eligible profile, or add `--account` flags to select specific UUIDs from the list. For a single profile, you can pass `--live --account UUID` and supply its token at the hidden prompt, from `NAMEMCSNIPER_TOKEN`, or from standard input with `--token-stdin`. [Command reference](../usage/commands.md)

Use a fresh token that will still be valid at preflight. If Minecraft returns HTTP 401, get a fresh token and check the account again. If the profile check fails, confirm the signed-in account has an existing Java Edition profile.
