<div align="center">

# 🛡️ Available functions 🛡️

</div>

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

---

## 📋 The launcher

These commands prepare, install, or inspect the workstation.

## ▪️ help

Lists the liltress commands.

```bash
./liltress help
```

Recognised commands: `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`.<br/>
`-h` and `--help` show the same help.<br/>
An unknown command prints the unknown-command line and exits `2`.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/share/core/run/launch.sh`

## ▪️ configure

Prepares the configuration, then installs.

```bash
sudo ./liltress configure
```

The script asks the questions, writes `global.conf` mode `644` and `secrets.conf` mode `600`, then starts `script/run.sh`.<br/>
The Ubuntu Pro token is required. Alerts are answered with `Y` or `n`.<br/>
When alerts are enabled, the SMTP server, the login, the application password and the From address are asked, and `swaks` must receive a `250`.

`MODE_TEST` is answered with `0` or `1`. The shipped default is `1`. An unknown option is refused.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Essai d'envoi accepté.`

Success : `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

Source : `src/workstation/script/configure.sh`

## ▪️ check

Checks `global.conf` and `secrets.conf` without rewriting them.

```bash
sudo ./liltress check
```

Exit code `0` means both files conform.<br/>
`secrets.conf` is mode `600`, so `sudo` is required to read it.<br/>
This is the same check as `script/configure.sh --check`.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Configuration conforme`

Source : `src/workstation/script/configure.sh`

## ▪️ run

Starts the installation from the files already prepared.

```bash
sudo ./liltress run
```

The installer refuses to start when the configuration check fails.<br/>
It then purges, installs packages, hardens, runs the security checks, builds the AIDE and rkhunter databases, then `verify/workstation.sh`.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/script/run.sh`

## ▪️ lynis

Runs a Lynis audit of the workstation.

```bash
sudo ./liltress lynis
```

The real command is `lynis audit system --quick --no-colors`.<br/>
The report is written under `<source>/log/lynis-<date>/`.<br/>
The last line shows `Rapport :` followed by that directory.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Rapport :`

Source : `src/share/core/run/lynis.sh`

## ▪️ logs

Copies a log snapshot into the sources.

```bash
sudo ./liltress logs
```

The directory created is `<source>/log/snapshot-<date>-<host>`.<br/>
Private files (`secrets.conf`, keys, passwords) are not copied.<br/>
A file over 8 MiB is truncated. The cut is noted in `snapshot/truncated.txt`.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/share/script/log-collect.sh`

## 📋 VPN

These commands manage profiles.

## ▪️ vpn status

Shows the VPN mode, the killswitch, and the active profile.

```bash
sudo ./liltress vpn status
```

The mode is `on` or `off`.<br/>
The killswitch is armed or disarmed.<br/>
When no profile is active, the line says so.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn list

Lists the registered VPN profiles.

```bash
sudo ./liltress vpn list
```

Each line gives the name, the protocol, the validity, the validation date, the observed date, and the date of the last try.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import wireguard <marque> <nom> <dossier ou fichier>

Imports a WireGuard profile.

```bash
sudo ./liltress vpn import wireguard <marque> <nom> <dossier ou fichier>
```

The file is copied under `/opt/liltress-workstation/vpn/wireguard/<nom>/`.<br/>
A try validates the profile.<br/>
The mode arms on the first valid profile.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import openvpn <marque> <nom> <dossier ou fichier>

Imports an OpenVPN profile.

```bash
sudo ./liltress vpn import openvpn <marque> <nom> <dossier ou fichier>
```

The files cited by the profile are copied.<br/>
An `auth-user-pass` with no path receives `auth.txt`.<br/>
The tunnel does not open when the credential file has fewer than two lines.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn test <nom>

Retests a profile and validates it again when it answers.

```bash
sudo ./liltress vpn test <nom>
```

Three dates are kept: validation, last observed state, last try.<br/>
A failure does not remove the validation date.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn switch <nom>

Switches to another profile that is already valid.

```bash
sudo ./liltress vpn switch <nom>
```

The command does not bring a profile down.<br/>
It only chooses a profile that is still valid.<br/>
Services that depend on it, including Unbound, are restarted.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode on

Arms VPN mode.

```bash
sudo ./liltress vpn mode on
```

The tunnel and the killswitch stay until a manual stop.<br/>
SSH does not enter the tunnel.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode off

Stops VPN mode.

```bash
sudo ./liltress vpn mode off
```

The network becomes direct and the killswitch is removed.<br/>
A later successful try does not arm the mode again.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn rm <nom>

Removes a VPN profile.

```bash
sudo ./liltress vpn rm <nom>
```

The active profile cannot be removed.<br/>
The command deletes the directory and the registry line.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass

Shows the prefixes that stay outside the tunnel.

```bash
sudo ./liltress vpn bypass
```

The list read is `/opt/liltress-workstation/vpn/bypass.cidrs`.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass reload

Reloads the prefixes that stay outside the tunnel.

```bash
sudo ./liltress vpn bypass reload
```

Bypass routes are applied again when the killswitch is armed.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn refresh-endpoint <nom>

Resolves the VPN server address again.

```bash
sudo ./liltress vpn refresh-endpoint <nom>
```

The profile DNS name is resolved once more.<br/>
The address replaces the stored endpoint.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/service/vpn/cli.sh`

Brands, the killswitch, and the startup profile are in [vpn.md](vpn.md).

## 📋 Checks

These commands check. They do not rebuild a baseline.

## ▪️ verify alerts

Checks the alert chain and sends one probe.

```bash
sudo ./liltress verify alerts
```

The pass checks the boot timer, the periodic timer, on-access ClamAV, and the mail path.<br/>
One test message is sent when alerts are enabled. The subject ends with `Contrôle`.<br/>
`-h` and `--help` show the usage. Any other option is refused.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Résultat : OK`

Source : `src/workstation/script/verify/alerts.sh`

## ▪️ verify alerts --no-send

Same alert check, with no probe.

```bash
sudo ./liltress verify alerts --no-send
```

Same scope as `verify/alerts.sh`.<br/>
No test message is sent.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Résultat : OK`

Source : `src/workstation/script/verify/alerts.sh`

## ▪️ verify workstation

Checks the workstation after installation.

```bash
sudo ./liltress verify workstation
```

The first argument is the sources directory.<br/>
The closing lines are `Check Success : N`, `Check Warnings : N` and `Check Fails : N`.<br/>
The pass also checks SSH forwarding, the SSH firewall rule, USBGuard, the sudo timeout, and `w-reboot-notice.timer`.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Check Success : N`

Source : `src/workstation/script/verify/workstation.sh`

## ▪️ verify boot-check

Checks the security services of this boot.

```bash
sudo ./liltress verify boot-check
```

A pass still running prints `Contrôle de démarrage : en cours` and exits `1`.<br/>
A gap prints `Contrôle de démarrage : écarts` and exits `1`.

It can run from any directory on the machine.

Success : `Contrôle de démarrage : OK`

Source : `src/workstation/script/boot-check.sh`

## ▪️ verify boot

Runs `boot-check.sh` from the sources.

```bash
sudo ./liltress verify boot
```

`script/verify/boot.sh` runs `script/boot-check.sh` from the same tree.<br/>
The result is the result of `boot-check.sh`.

Run it from `/opt/liltress-workstation`, or from the clone.

Success : `Contrôle de démarrage : OK`

Source : `src/workstation/script/verify/boot.sh`

## ▪️ verify chkrootkit

Reads a chkrootkit pass for a human.

```bash
sudo ./liltress verify chkrootkit
```

The journal is `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`.<br/>
This command writes nothing to the ignore list.

Run it from `/opt/liltress-workstation`, or from the clone.

Source : `src/workstation/script/verify/chkrootkit-fp.sh`

## 📋 Maintenance

These commands rebuild a baseline, record an ignore list, or restore a file.

## ▪️ maintain aide

Rebuilds the AIDE database.

```bash
sudo ./liltress maintain aide
```

The command runs `aide --init` and replaces `/var/lib/aide/aide.db`.<br/>
The calculation can take several minutes.<br/>
This is the command attached to the AIDE mail.

It can run from any directory on the machine.

Success : `Base de référence AIDE enregistrée.`

Source : `src/workstation/service/aide/refresh-db.sh`

Installed : `/opt/liltress-workstation/service/aide/refresh-db.sh`

## ▪️ maintain rkhunter

Rebuilds the rkhunter property database.

```bash
sudo ./liltress maintain rkhunter
```

The command runs `rkhunter --propupd --nocolors`.<br/>
This is the command attached to the rkhunter warning mail.<br/>
The mail `mirrors.dat modifié hors rkhunter --update` does not attach it.

It can run from any directory on the machine.

Success : `Base de propriétés rkhunter enregistrée.`

Source : `src/share/service/rkhunter/refresh-db.sh`

Installed : `/opt/liltress-workstation/service/rkhunter/refresh-db.sh`

## ▪️ maintain chkrootkit <journal>

Records lines of a chkrootkit journal as ignored.

```bash
sudo ./liltress maintain chkrootkit <journal>
```

The file must be a `chkrootkit-DATE.log` already under `cron/security_logs/`.<br/>
New lines are appended to `/opt/liltress-workstation/cron/chkrootkit.local.ignore`.<br/>
When nothing is new, the line is `Aucune nouvelle détection à enregistrer.` Otherwise it names the count recorded in that file.

It can run from any directory on the machine.

Success : `Aucune nouvelle détection à enregistrer.`

Success : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`

Source : `src/workstation/service/chkrootkit/ignore-log.sh`

Installed : `/opt/liltress-workstation/service/chkrootkit/ignore-log.sh`

## ▪️ maintain debsums <journal>

Records lines of a debsums journal as ignored.

```bash
sudo ./liltress maintain debsums <journal>
```

The file must be a `debsums-DATE.log` under `cron/security_logs/`.<br/>
The patterns go into `/opt/liltress-workstation/cron/debsums.local.ignore`.<br/>
The success lines are the same as for chkrootkit. The debsums mail does not offer this command: the check already reinstalls each package still reported.

It can run from any directory on the machine.

Success : `Aucune nouvelle détection à enregistrer.`

Success : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/debsums.local.ignore`

Source : `src/workstation/service/debsums/ignore-log.sh`

Installed : `/opt/liltress-workstation/service/debsums/ignore-log.sh`

## ▪️ maintain scan-skip list

Lists paths excluded from ClamAV on-access.

```bash
sudo ./liltress maintain scan-skip list
```

One accepted path is printed per line.<br/>
A refused path is written on stderr: `Chemin refusé : <path>`.<br/>
The file is `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`.

It can run from any directory on the machine.

Source : `src/workstation/script/scan-skip.sh`

Installed : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip add <chemin>

Adds a path excluded from ClamAV on-access.

```bash
sudo ./liltress maintain scan-skip add <chemin>
```

Already present: `Ce chemin est déjà exclu : <path>`.<br/>
A refusal leaves the file unchanged and prints `Chemin refusé : <path>`.<br/>
A path is accepted under the workstation account home, and below that home.

`/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, the whole home, Downloads, Desktop, and their parent are refused.<br/>
After a change, the command rewrites the exclusion block of `/etc/clamav/clamd.conf`, restarts `clamav-daemon`, then `clamav-clamonacc.service` or `clamonacc.service`.

It can run from any directory on the machine.

Success : `Chemin ajouté : <path>`

Source : `src/workstation/script/scan-skip.sh`

Installed : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip del <chemin>

Removes a path from the ClamAV exclusion list.

```bash
sudo ./liltress maintain scan-skip del <chemin>
```

When it is absent, the command prints `Ce chemin n'est pas dans la liste : <path>` and exits `1`.<br/>
The same `clamd.conf` rewrite follows a successful removal.

It can run from any directory on the machine.

Success : `Chemin retiré : <path>`

Source : `src/workstation/script/scan-skip.sh`

Installed : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain mirrors save

Records the rkhunter mirror fingerprint.

```bash
sudo ./liltress maintain mirrors save
```

The SHA-256 of `/var/lib/rkhunter/db/mirrors.dat` is written to `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, mode `600`.<br/>
`refresh-rkhunter-db.sh` does this only after `rkhunter --update` returns `0` or `2`.

It can run from any directory on the machine.

Source : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Installed : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain mirrors check

Compares the current rkhunter fingerprint with the saved one.

```bash
sudo ./liltress maintain mirrors check
```

Nothing is printed.<br/>
Exit `0` means the two fingerprints are equal, or that none is saved yet.<br/>
A difference exits with a code other than `0`.

It can run from any directory on the machine.

Source : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Installed : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain restore <id>

Puts a quarantine file back in its original place.

```bash
sudo ./liltress maintain restore <id>
```

The id is a line of `cron/quarantine.registry`.<br/>
The script is `core/quarantine/restore.sh`.

It can run from any directory on the machine.

Source : `src/share/core/quarantine/restore.sh`

Installed : `/opt/liltress-workstation/core/quarantine/restore.sh`

## ▪️ maintain mail

Sends a test mail.

```bash
sudo ./liltress maintain mail
```

The message is there to see the width and the height.<br/>
The recipient stays the one from the installed configuration.<br/>
The script is `core/mail/mail-test.sh`.

It can run from any directory on the machine.

Source : `src/share/core/mail/mail-test.sh`

Installed : `/opt/liltress-workstation/core/mail/mail-test.sh`
