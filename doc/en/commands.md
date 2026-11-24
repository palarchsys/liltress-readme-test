<div align="center">

# 🛡️ Commands after installation 🛡️

</div>

**English** · [Français](../fr/commands.md) · [Deutsch](../de/commands.md) · [Español](../es/commands.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Software and services](software.md)
- [Available functions](functions.md)
- [Alert emails](alerts.md)
- **Commands after installation**
- [Monitored zones](zones.md)
- [VPN](vpn.md)

---

Run these commands from `/opt/liltress-workstation`.

## 📋 Check the configuration

```bash
sudo ./liltress check
```

Checks `global.conf` and `secrets.conf`.<br/>
The success line is `Configuration conforme`.

## 📋 Run the installer again

```bash
sudo ./liltress run
```

Starts from the prepared files.<br/>
The installer refuses to start when the check does not pass.

## 📋 Probe the alert path

```bash
sudo ./liltress verify alerts
```

Checks the boot-chain timer, the periodic timer, on-access ClamAV, and the mail path.<br/>
Without `--no-send`, one probe is sent when alerts are enabled.<br/>
The subject ends with `Contrôle`.

The success line is `Résultat : OK`.

```bash
sudo ./liltress verify alerts --no-send
```

Same check. No probe.

## 📋 Check the workstation

```bash
sudo ./liltress verify workstation
```

Closing lines: `Check Success : N`, `Check Warnings : N`, `Check Fails : N`.<br/>
The same pass checks SSH forwarding, the SSH firewall rule, USBGuard, the sudo timeout, and `w-reboot-notice.timer`.

## 📋 Check this boot

```bash
sudo ./liltress verify boot
```

Success: `Contrôle de démarrage : OK`.<br/>
Still running: `Contrôle de démarrage : en cours`, exit `1`.<br/>
A gap: `Contrôle de démarrage : écarts`, exit `1`.

## 📋 Lynis

```bash
sudo ./liltress lynis
```

Runs `lynis audit system --quick --no-colors`.<br/>
The report is under `<source>/log/lynis-<stamp>/`.<br/>
`LYNIS_MIN_SCORE` is 80.

## 📋 Rebuild baselines

```bash
sudo ./liltress maintain aide
sudo ./liltress maintain rkhunter
```

AIDE success: `Base de référence AIDE enregistrée.`<br/>
rkhunter success: `Base de propriétés rkhunter enregistrée.`

## 📋 rkhunter mirror stamp

```bash
sudo ./liltress maintain mirrors save
sudo ./liltress maintain mirrors check
```

`mirrors save` writes the SHA-256 to `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, mode `600`.<br/>
`mirrors check` prints nothing. Exit `0` means the fingerprints match, or that none is saved yet.

## 📋 Excluded on-access paths

```bash
sudo ./liltress maintain scan-skip list
sudo ./liltress maintain scan-skip add $HOME/.cache/exemple
sudo ./liltress maintain scan-skip del $HOME/.cache/exemple
```

| Action | Printed line |
| --- | --- |
| `list` | one accepted path per line |
| `add` | `Chemin ajouté : <path>` |
| already there | `Ce chemin est déjà exclu : <path>` |
| `del` | `Chemin retiré : <path>` |
| refused | `Chemin refusé : <path>` |

The full command list, including VPN and quarantine restore, is in [functions.md](functions.md).
