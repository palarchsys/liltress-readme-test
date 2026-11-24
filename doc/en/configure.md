# Configuration

**English** · [Français](../fr/configure.md) · [Deutsch](../de/configure.md) · [Español](../es/configure.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- **Configuration**
- [Software and services](software.md)
- [Available functions](functions.md)
- [Alert emails](alerts.md)
- [Commands after installation](commands.md)
- [Monitored zones](zones.md)
- [VPN](vpn.md)

`sudo ./liltress configure` asks the questions below, writes `global.conf` mode `644` and `secrets.conf` mode `600` as root, then starts `script/run.sh`.

The first question chooses the language.

The value is `LANGUAGE`.

The default is English.

`MODE_TEST` is answered with `0` or `1`. The shipped default is `1`.

An unknown option is refused.

| Question | Expected answer |
| --- | --- |
| Ubuntu Pro token | Letters and digits, 6 to 100, no space. Required. |
| Enable mail | `Y` or `y` enables mail. `N`, `n` or `non` disables mail. Disabled does not install Postfix. Any other answer prints `Répondez Y ou N.` |
| Server | `host:port`, for example `smtp-mail.outlook.com:587`. No brackets. Asked only when alerts are enabled. |
| Login | An e-mail address. |
| Password | The provider application password, 8 to 128 characters. Spaces are removed. Quotes, `$`, `\` and backticks are refused. Never the account password. |
| From | An e-mail address. `WATCHDOG_MAIL` receives the same address. |

When alerts are enabled, `swaks` must receive a `250`.

The success line of that test is `Essai d'envoi accepté.`

When alerts stay off, the line is `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

## Check without rewriting

```bash
sudo ./liltress check
```

The command checks `global.conf` and `secrets.conf` and does not rewrite them.

`secrets.conf` is mode `600`, so `sudo` is required to read it.

Exit code `0` means both files conform.

The success line is `Configuration conforme`.

This is the same check as `script/configure.sh --check`.
