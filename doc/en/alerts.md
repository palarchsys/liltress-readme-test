<div align="center">

# 🛡️ Alert emails 🛡️

</div>

**English** · [Français](../fr/alerts.md) · [Deutsch](../de/alerts.md) · [Español](../es/alerts.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Software and services](software.md)
- [Available functions](functions.md)
- **Alert emails**
- [Commands after installation](commands.md)
- [Monitored zones](zones.md)
- [VPN](vpn.md)

---

Each alert file is stored under `/opt/liltress-workstation/cron/alerts`.<br/>
The mail follows `LANGUAGE`.<br/>
Mail leaves only when `MAIL_ALERTS=1`, `WATCHDOG_MAIL` is set, and `/opt/liltress-workstation/service/cron/watchdogs/send.sh` is executable.

A clean check does not send mail.<br/>
If a journal line is unknown, do not run the command. Keep the mail.<br/>
A check that stops before its normal end sends mail. The mail names the check, and the signal or the exit code.

## 📋 AIDE

The message contains the summary, a journal excerpt, a verification block for each finding, then the note and the command.<br/>
The journal form is `/opt/liltress-workstation/cron/security_logs/aide-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain aide
```

The command rebuilds the AIDE database from the current disk.<br/>
The success line is `Base de référence AIDE enregistrée.`

## 📋 chkrootkit

The message contains the summary, the excerpt, a verification block per finding, then the note and the command.

```bash
sudo ./liltress maintain chkrootkit '/opt/liltress-workstation/cron/security_logs/chkrootkit-YYYYMMDD-HHMMSS.log'
```

The command appends new lines to `/opt/liltress-workstation/cron/chkrootkit.local.ignore`.<br/>
Success is `Aucune nouvelle détection à enregistrer.` or `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`.<br/>
A confirmed detection moves the file and attaches the restore command instead. The record command stays only when no confirmed file was moved.

## 📋 debsums

The message contains the summary and the excerpt. It does not add a command block.<br/>
The check reinstalls each package still reported with `apt-get install --reinstall`.<br/>
The mail names each package restored.

The success line is `La somme correspond.`<br/>
A package whose checksum stays different, or whose reinstall fails, stays in the mail.<br/>
A line for `/var/lib/rkhunter/db/mirrors.dat` is removed before that reinstall only when its current SHA-256 equals `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`.

A changed line under `/usr/lib/*/perl-base/unicore/` of the `perl-base` package is removed before that reinstall. The file shipped by Ubuntu does not match that package checksum.

## 📋 ClamAV on access

The message contains the signature, the file name, the original path, the quarantine path, the time, the detection type, the actions, a short recommendation, and one restore command per moved file.<br/>
A file whose name matches `.clamav-quarantine-lock.*` does not send mail. The script exits `0` before the journal and before `log_alert`.

```bash
sudo bash /opt/liltress-workstation/core/quarantine/restore.sh <id>
```

The success line is `fichier remis en place`.<br/>
The registry is `/var/lib/liltress-workstation/watchdog/quarantine.registry`.

## 📋 ClamAV boot scan and periodic scan

Both messages contain the summary and the journal excerpt. A moved file adds one restore command, the same command as on-access.<br/>
The journal form is `/opt/liltress-workstation/cron/security_logs/clamav-YYYYMMDD-HHMMSS.log`.<br/>
The periodic chain runs every 1 hour. The same scan also runs once in the boot chain.

The periodic walk includes paths excluded from the on-access scan.<br/>
An unreadable file, with no `FOUND` line, does not send mail. The lines `Known viruses` and `Infected files` are not copied into the mail.<br/>
A `clamscan` stopped by a signal sends mail. The mail names the signal and each unread file. The other files of the pass are scanned. Two crashes in a row stop the ClamAV pass. The mail then says that the remaining files were not read.

## 📋 rkhunter

The warning mail contains the summary, the excerpt, a verification block per finding, then the note and the command.<br/>
The mail `mirrors.dat modifié hors rkhunter --update` does not add that block. An error mail does not either.<br/>
The journal form is `/opt/liltress-workstation/cron/security_logs/rkhunter-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain rkhunter
```

The success line is `Base de propriétés rkhunter enregistrée.`<br/>
A confirmed detection moves the file and attaches the restore command instead.<br/>
The files in `/dev/shm` and the socket `/dev/log` are allowed in `rkhunter.conf.local`. They do not produce `Suspicious file types found in /dev`.

## 📋 Reboot notice

The message is `Un redémarrage est utile. La machine n'a pas été redémarrée.`<br/>
This mail does not add a command block.<br/>
The same set of packages does not send mail again.

The script does not reboot the machine.<br/>
The journal form is `/opt/liltress-workstation/cron/security_logs/reboot-notice-YYYYMMDD-HHMMSS.log`.
