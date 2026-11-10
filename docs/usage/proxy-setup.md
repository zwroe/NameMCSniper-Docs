# Test a proxy list

The proxy tester checks whether an HTTP or HTTPS proxy can reach Minecraft Services over HTTPS. Live name changes and account checks use a direct connection.

## Create the list

Create a UTF-8 file called `proxies.txt` and add one proxy per line. The app accepts an HTTP or HTTPS URL, a bare `host:port`, or the provider format `host:port@username:password`. Both forms without a scheme use HTTP. Blank lines and lines starting with `#` are ignored.

```text
# Replace these example entries with your own
proxy.example:8080
https://proxy.example:8443
http://username:example-password@proxy.example:8080
proxy.example:8080@username:example-password
```

Percent-encode reserved characters in the username or password. For example, write `%40` for an `@` in a password. The list supports up to 256 entries and 64 KiB.

## Run the check

In the menu, choose **Test proxies**. The default is `proxies.txt` in the current folder if it exists, then `proxies.txt` beside the executable. Press Enter to use the displayed path or enter a path to your list. Choose **1** to save passing entries to `proxies_working.txt` next to the input file. Press Enter or **0** to test without exporting, or enter a different output filename.

From a shell, run one of these commands.

```text
namemcsniper test-proxies
namemcsniper test-proxies proxies.txt --output proxies_working.txt
```

The shell command looks in the current folder when you omit the input filename. Choose a new file for the export. It keeps the original entries, including credentials, so keep the exported list private.

## Read the results

The tester sends an unauthenticated HTTPS profile request through each proxy. The expected `401 Unauthorized` response passes the reachability check. The terminal shows redacted addresses and elapsed time for each result.

To schedule a name change, follow the [menu guide](basic-usage.md).
