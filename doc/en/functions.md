# Available functions

**English** · [Français](../fr/functions.md) · [Deutsch](../de/functions.md) · [Español](../es/functions.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Software and services](software.md)
- **Available functions**
- [Alert emails](alerts.md)
- [Commands after installation](commands.md)
- [Monitored zones](zones.md)
- [VPN](vpn.md)

Each command below is a real `./liltress` entry. Run them from `/opt/liltress-workstation` unless the row says otherwise.

Success text is the line the script prints. It is not translated.

## liltress

| Command | Effect | Success or exit |
| --- | --- | --- |
| `./liltress help` | Lists the recognised commands: `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`. `-h` and `--help` show the same help. | An unknown command prints the unknown-command line and exits `2`. |
| `sudo ./liltress configure` | Asks the questions, writes `global.conf` mode `644` and `secrets.conf` mode `600`, then starts `script/run.sh`. | Mail test: `Essai d'envoi accepté.` Mail off: `Alertes e-mail coupées. Les contrôles partiront sans envoi.` |
| `sudo ./liltress check` | Checks `global.conf` and `secrets.conf` without rewriting them. Same as `script/configure.sh --check`. | `Configuration conforme` |
| `sudo ./liltress run` | Starts the installation from the prepared files. Refuses to start when the configuration check fails. Purges, installs packages, hardens, runs the security checks, builds the AIDE and rkhunter databases, then `verify workstation`. | The installer stops if the check fails. |
| `sudo ./liltress lynis` | Runs `lynis audit system --quick --no-colors`. Writes the report under `<source>/log/lynis-<date>/`. | Last line starts with `Rapport :` |
| `sudo ./liltress logs` | Copies a log snapshot to `<source>/log/snapshot-<date>-<host>`. Private files (`secrets.conf`, keys, passwords) are not copied. A file over 8 MiB is truncated and the cut is noted in `snapshot/truncated.txt`. | The snapshot directory is the result. |

## vpn

| Command | Effect |
| --- | --- |
| `sudo ./liltress vpn status` | Shows mode `on` or `off`, whether the killswitch is armed, and the active profile. |
| `sudo ./liltress vpn list` | Lists registered profiles: name, protocol, validity, validation date, observed date, last try. |
| `sudo ./liltress vpn import wireguard <brand> <name> <directory or file>` | Copies the profile under `/opt/liltress-workstation/vpn/wireguard/<name>/` and tests it. The first valid profile arms the mode. |
| `sudo ./liltress vpn import openvpn <brand> <name> <directory or file>` | Copies the cited files. An `auth-user-pass` with no path receives `auth.txt`. The tunnel does not open when the credential file has fewer than two lines. |
| `sudo ./liltress vpn test <name>` | Retests a profile. A failure does not remove the validation date. |
| `sudo ./liltress vpn switch <name>` | Moves to another profile that is still valid. Does not bring a profile down. Restarts services that depend on it, including Unbound. |
| `sudo ./liltress vpn mode on` | Arms VPN mode. The tunnel and the killswitch stay until a manual stop. SSH does not enter the tunnel. |
| `sudo ./liltress vpn mode off` | Stops VPN mode. The network becomes direct and the killswitch is removed. A later successful test does not re-arm the mode. |
| `sudo ./liltress vpn rm <name>` | Removes a profile. The active profile cannot be removed. Deletes the directory and the registry line. |
| `sudo ./liltress vpn bypass` | Prints prefixes that stay outside the tunnel. The file is `/opt/liltress-workstation/vpn/bypass.cidrs`. |
| `sudo ./liltress vpn bypass reload` | Reloads those prefixes. Bypass routes are applied again when the killswitch is armed. |
| `sudo ./liltress vpn refresh-endpoint <name>` | Resolves the profile DNS name again and replaces the stored endpoint. |
| `sudo ./liltress vpn default` | Shows the startup profile, or none. |
| `sudo ./liltress vpn default <name>` | Selects a valid profile as the startup VPN. The current session stays as it is. |
| `sudo ./liltress vpn default off` | Clears that choice. An open tunnel stays up. |

Detail of import rules, brands, and killswitch behaviour is in [vpn.md](vpn.md).

## verify

| Command | Effect | Success or exit |
| --- | --- | --- |
| `sudo ./liltress verify alerts` | Checks the boot timer, the periodic timer, on-access ClamAV, and the mail path. Sends one probe when alerts are enabled. The subject ends with `Contrôle`. | `Résultat : OK` |
| `sudo ./liltress verify alerts --no-send` | Same check. No probe is sent. | `Résultat : OK` |
| `sudo ./liltress verify workstation` | Post-install check. Also checks SSH forwarding, the SSH firewall rule, USBGuard, the sudo timeout, and `w-reboot-notice.timer`. | `Check Success : N`, `Check Warnings : N`, `Check Fails : N` |
| `sudo ./liltress verify boot` | Runs `script/boot-check.sh` from the sources. | `Contrôle de démarrage : OK` |
| `sudo ./liltress verify boot-check` | Same boot check from any directory on the target machine. | Still running: `Contrôle de démarrage : en cours`, exit `1`. Gap: `Contrôle de démarrage : écarts`, exit `1`. |
| `sudo ./liltress verify chkrootkit` | Reads a chkrootkit pass for a human. Journal: `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`. | Does not write the ignore list. |

## maintain

| Command | Effect | Success line |
| --- | --- | --- |
| `sudo ./liltress maintain aide` | Runs `aide --init` and replaces `/var/lib/aide/aide.db`. Can take several minutes. | `Base de référence AIDE enregistrée.` |
| `sudo ./liltress maintain rkhunter` | Runs `rkhunter --propupd --nocolors`. | `Base de propriétés rkhunter enregistrée.` |
| `sudo ./liltress maintain chkrootkit <journal>` | Records new lines from a `chkrootkit-DATE.log` already under `cron/security_logs/`. | `Aucune nouvelle détection à enregistrer.` or `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore` |
| `sudo ./liltress maintain debsums <journal>` | Records lines from a `debsums-DATE.log` into `/opt/liltress-workstation/cron/debsums.local.ignore`. The debsums mail does not offer this command: the check already reinstalls each package still reported. | Same two success forms, with `debsums.local.ignore`. |
| `sudo ./liltress maintain scan-skip list` | Lists paths excluded from ClamAV on-access. File: `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`. | One accepted path per line. Refused path on stderr: `Chemin refusé : <path>`. |
| `sudo ./liltress maintain scan-skip add <path>` | Adds one exclusion, rewrites the exclusion block of `/etc/clamav/clamd.conf`, restarts `clamav-daemon`, then `clamav-clamonacc.service` or `clamonacc.service`. | `Chemin ajouté : <path>` |
| `sudo ./liltress maintain scan-skip del <path>` | Removes one exclusion and rewrites `clamd.conf` the same way. | `Chemin retiré : <path>` |
| `sudo ./liltress maintain mirrors save` | Writes the SHA-256 of `/var/lib/rkhunter/db/mirrors.dat` to `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, mode `600`. | No fixed banner. The file is the result. |
| `sudo ./liltress maintain mirrors check` | Compares the current fingerprint with the saved one. Prints nothing. | Exit `0` when equal, or when no fingerprint is saved yet. Any other exit means a difference. |
| `sudo ./liltress maintain restore <id>` | Restores one quarantine file. The id is a line of `cron/quarantine.registry`. Script: `core/quarantine/restore.sh`. | `fichier remis en place` |
| `sudo ./liltress maintain mail` | Sends a layout test to the configured recipient. Script: `core/mail/mail-test.sh`. | The test message is the result. |

### scan-skip limits

A path is accepted under the workstation account home, and below that home.

`/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, the whole home, Downloads, Desktop, and their parent are refused.

Already present: `Ce chemin est déjà exclu : <path>`.

Absent on delete: `Ce chemin n'est pas dans la liste : <path>`, exit `1`.

A refused path leaves the file unchanged.
