# Alarm-E-Mails

[English](../en/alerts.md) · [Français](../fr/alerts.md) · **Deutsch** · [Español](../es/alerts.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Konfiguration](configure.md)
- [Software und Dienste](software.md)
- [Verfügbare Funktionen](functions.md)
- **Alarm-E-Mails**
- [Befehle nach der Installation](commands.md)
- [Überwachte Bereiche](zones.md)
- [VPN](vpn.md)

Jede Alarmdatei liegt unter `/opt/liltress-workstation/cron/alerts`.

Die Mail folgt `LANGUAGE`.

Mail geht nur hinaus, wenn `MAIL_ALERTS=1` gilt, `WATCHDOG_MAIL` gesetzt ist und `/opt/liltress-workstation/service/cron/watchdogs/send.sh` ausführbar ist.

Eine saubere Prüfung sendet keine Mail.

Ist eine Journalzeile unbekannt, führen Sie den Befehl nicht aus. Behalten Sie die Mail.

Eine Prüfung, die vor dem normalen Ende stoppt, sendet Mail. Die Mail nennt die Prüfung und das Signal oder den Exit-Code.

## AIDE

Die Nachricht enthält die Zusammenfassung, einen Journalauszug, einen Prüfblock je Fund, dann den Hinweis und den Befehl.

Die Journalform ist `/opt/liltress-workstation/cron/security_logs/aide-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain aide
```

Der Befehl baut die AIDE-Datenbank vom aktuellen Datenträger neu auf.

Die Erfolgszeile ist `Base de référence AIDE enregistrée.`

## chkrootkit

Die Nachricht enthält die Zusammenfassung, den Auszug, einen Prüfblock je Fund, dann den Hinweis und den Befehl.

```bash
sudo ./liltress maintain chkrootkit '/opt/liltress-workstation/cron/security_logs/chkrootkit-YYYYMMDD-HHMMSS.log'
```

Der Befehl hängt neue Zeilen an `/opt/liltress-workstation/cron/chkrootkit.local.ignore` an.

Erfolg ist `Aucune nouvelle détection à enregistrer.` oder `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`.

Ein bestätigter Fund verschiebt die Datei und hängt stattdessen den Wiederherstellungsbefehl an. Der Aufnahmebefehl bleibt nur, wenn keine bestätigte Datei verschoben wurde.

## debsums

Die Nachricht enthält die Zusammenfassung und den Auszug. Sie fügt keinen Befehlsblock an.

Die Prüfung installiert jedes weiter gemeldete Paket mit `apt-get install --reinstall` neu.

Die Mail nennt jedes wiederhergestellte Paket.

Die Erfolgszeile ist `La somme correspond.`

Ein Paket, dessen Prüfsumme verschieden bleibt oder dessen Neuinstallation scheitert, bleibt in der Mail.

Eine Zeile für `/var/lib/rkhunter/db/mirrors.dat` wird vor dieser Neuinstallation nur entfernt, wenn ihr aktueller SHA-256 gleich `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256` ist.

Eine geänderte Zeile unter `/usr/lib/*/perl-base/unicore/` des Pakets `perl-base` wird vor dieser Neuinstallation entfernt. Die von Ubuntu gelieferte Datei entspricht dieser Paketprüfsumme nicht.

## ClamAV beim Zugriff

Die Nachricht enthält die Signatur, den Dateinamen, den Ursprungspfad, den Quarantänepfad, die Zeit, die Art des Funds, die Aktionen, eine kurze Empfehlung und einen Wiederherstellungsbefehl je verschobener Datei.

Eine Datei, deren Name `.clamav-quarantine-lock.*` entspricht, sendet keine Mail. Das Skript endet mit `0` vor dem Journal und vor `log_alert`.

```bash
sudo bash /opt/liltress-workstation/core/quarantine/restore.sh <id>
```

Die Erfolgszeile ist `fichier remis en place`.

Das Register ist `/var/lib/liltress-workstation/watchdog/quarantine.registry`.

## ClamAV beim Start und periodischer Scan

Beide Nachrichten enthalten die Zusammenfassung und den Journalauszug. Eine verschobene Datei fügt einen Wiederherstellungsbefehl hinzu, denselben Befehl wie beim Zugriff.

Die Journalform ist `/opt/liltress-workstation/cron/security_logs/clamav-YYYYMMDD-HHMMSS.log`.

Die periodische Kette läuft jede Stunde. Derselbe Scan läuft auch einmal in der Startkette.

Der periodische Lauf geht auch über Pfade, die vom Zugriffsscan ausgenommen sind.

Eine unlesbare Datei ohne Zeile `FOUND` sendet keine Mail. Die Zeilen `Known viruses` und `Infected files` werden nicht in die Mail kopiert.

Ein durch ein Signal gestopptes `clamscan` sendet Mail. Die Mail nennt das Signal und jede ungelesene Datei. Die übrigen Dateien des Laufs werden gelesen. Zwei Abstürze hintereinander stoppen den ClamAV-Lauf. Die Mail sagt dann, dass die übrigen Dateien nicht gelesen wurden.

## rkhunter

Die Warnmail enthält die Zusammenfassung, den Auszug, einen Prüfblock je Fund, dann den Hinweis und den Befehl.

Die Mail `mirrors.dat modifié hors rkhunter --update` fügt diesen Block nicht an. Eine Fehlermail ebenfalls nicht.

Die Journalform ist `/opt/liltress-workstation/cron/security_logs/rkhunter-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain rkhunter
```

Die Erfolgszeile ist `Base de propriétés rkhunter enregistrée.`

Ein bestätigter Fund verschiebt die Datei und hängt stattdessen den Wiederherstellungsbefehl an.

Die Dateien in `/dev/shm` und der Socket `/dev/log` sind in `rkhunter.conf.local` erlaubt. Sie erzeugen kein `Suspicious file types found in /dev`.

## Hinweis zum Neustart

Die Nachricht ist `Un redémarrage est utile. La machine n'a pas été redémarrée.`

Diese Mail fügt keinen Befehlsblock an.

Derselbe Paketsatz sendet die Mail nicht erneut.

Das Skript startet die Maschine nicht neu.

Die Journalform ist `/opt/liltress-workstation/cron/security_logs/reboot-notice-YYYYMMDD-HHMMSS.log`.
