<div align="center">

# 🛡️ Verfügbare Funktionen 🛡️

</div>

[English](../en/functions.md) · [Français](../fr/functions.md) · **Deutsch** · [Español](../es/functions.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Konfiguration](configure.md)
- [Software und Dienste](software.md)
- **Verfügbare Funktionen**
- [Alarm-E-Mails](alerts.md)
- [Befehle nach der Installation](commands.md)
- [Überwachte Bereiche](zones.md)
- [VPN](vpn.md)

---

## 📋 Der Starter

Diese Befehle bereiten vor, installieren oder prüfen den Arbeitsplatz.

## ▪️ help

Listet die liltress-Befehle.

```bash
./liltress help
```

Erkannte Befehle: `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`.<br/>
`-h` und `--help` zeigen dieselbe Hilfe.<br/>
Ein unbekannter Befehl druckt die Zeile des unbekannten Befehls und endet mit `2`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/share/core/run/launch.sh`

## ▪️ configure

Bereitet die Konfiguration vor und installiert dann.

```bash
sudo ./liltress configure
```

Das Skript stellt die Fragen, schreibt `global.conf` Modus `644` und `secrets.conf` Modus `600`, und startet dann `script/run.sh`.<br/>
Der Ubuntu-Pro-Token ist Pflicht. Alarme werden mit `Y` oder `n` beantwortet.<br/>
Sind Alarme aktiv, werden SMTP-Server, Login, Anwendungspasswort und From-Adresse gefragt, und `swaks` muss ein `250` erhalten.

`MODE_TEST` wird mit `0` oder `1` beantwortet. Der gelieferte Vorgabewert ist `1`. Eine unbekannte Option wird abgelehnt.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Essai d'envoi accepté.`

Erfolg : `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

Quelle : `src/workstation/script/configure.sh`

## ▪️ check

Prüft `global.conf` und `secrets.conf`, ohne sie neu zu schreiben.

```bash
sudo ./liltress check
```

Exit-Code `0` bedeutet, dass beide Dateien stimmen.<br/>
`secrets.conf` hat Modus `600`, deshalb ist `sudo` zum Lesen nötig.<br/>
Das ist dieselbe Prüfung wie `script/configure.sh --check`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Configuration conforme`

Quelle : `src/workstation/script/configure.sh`

## ▪️ run

Startet die Installation aus den schon vorbereiteten Dateien.

```bash
sudo ./liltress run
```

Der Installer verweigert den Start, wenn die Konfigurationsprüfung scheitert.<br/>
Danach folgen Bereinigung, Pakete, Härtung, Sicherheitsprüfungen, die Basen von AIDE und rkhunter, dann `verify/workstation.sh`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/script/run.sh`

## ▪️ lynis

Führt ein Lynis-Audit des Arbeitsplatzes aus.

```bash
sudo ./liltress lynis
```

Der echte Befehl ist `lynis audit system --quick --no-colors`.<br/>
Der Bericht liegt unter `<source>/log/lynis-<date>/`.<br/>
Die letzte Zeile zeigt `Rapport :` und dann dieses Verzeichnis.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Rapport :`

Quelle : `src/share/core/run/lynis.sh`

## ▪️ logs

Kopiert einen Journal-Schnappschuss in die Quellen.

```bash
sudo ./liltress logs
```

Das angelegte Verzeichnis ist `<source>/log/snapshot-<date>-<host>`.<br/>
Private Dateien (`secrets.conf`, Schlüssel, Passwörter) werden nicht kopiert.<br/>
Eine Datei über 8 MiB wird gekürzt. Der Schnitt steht in `snapshot/truncated.txt`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/share/script/log-collect.sh`

## 📋 VPN

Diese Befehle verwalten Profile.

## ▪️ vpn status

Zeigt den VPN-Modus, den Killswitch und das aktive Profil.

```bash
sudo ./liltress vpn status
```

Der Modus ist `on` oder `off`.<br/>
Der Killswitch ist scharf oder nicht.<br/>
Ohne aktives Profil sagt die Zeile das.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn list

Listet die eingetragenen VPN-Profile.

```bash
sudo ./liltress vpn list
```

Jede Zeile nennt Name, Protokoll, Gültigkeit, Validierungsdatum, beobachtetes Datum und das Datum des letzten Versuchs.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import wireguard <marque> <nom> <dossier ou fichier>

Importiert ein WireGuard-Profil.

```bash
sudo ./liltress vpn import wireguard <marque> <nom> <dossier ou fichier>
```

Die Datei wird nach `/opt/liltress-workstation/vpn/wireguard/<nom>/` kopiert.<br/>
Ein Versuch validiert das Profil.<br/>
Der Modus wird beim ersten gültigen Profil scharf.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import openvpn <marque> <nom> <dossier ou fichier>

Importiert ein OpenVPN-Profil.

```bash
sudo ./liltress vpn import openvpn <marque> <nom> <dossier ou fichier>
```

Die vom Profil genannten Dateien werden kopiert.<br/>
Ein `auth-user-pass` ohne Pfad erhält `auth.txt`.<br/>
Der Tunnel öffnet nicht, wenn die Zugangsdatei weniger als zwei Zeilen hat.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn test <nom>

Prüft ein Profil erneut und validiert es wieder, wenn es antwortet.

```bash
sudo ./liltress vpn test <nom>
```

Drei Daten werden geführt: Validierung, letzter beobachteter Zustand, letzter Versuch.<br/>
Ein Fehlschlag entfernt das Validierungsdatum nicht.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn switch <nom>

Wechselt zu einem anderen bereits gültigen Profil.

```bash
sudo ./liltress vpn switch <nom>
```

Der Befehl fährt kein Profil herunter.<br/>
Er wählt nur ein noch gültiges Profil.<br/>
Dienste, die davon abhängen, darunter Unbound, werden neu gestartet.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode on

Schärft den VPN-Modus.

```bash
sudo ./liltress vpn mode on
```

Tunnel und Killswitch bleiben bis zu einem manuellen Stopp.<br/>
SSH geht nicht in den Tunnel.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode off

Stoppt den VPN-Modus.

```bash
sudo ./liltress vpn mode off
```

Das Netz wird direkt und der Killswitch wird entfernt.<br/>
Ein späterer erfolgreicher Versuch schärft den Modus nicht erneut.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn rm <nom>

Entfernt ein VPN-Profil.

```bash
sudo ./liltress vpn rm <nom>
```

Das aktive Profil kann nicht entfernt werden.<br/>
Der Befehl löscht das Verzeichnis und die Registerzeile.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass

Zeigt die Präfixe, die außerhalb des Tunnels bleiben.

```bash
sudo ./liltress vpn bypass
```

Gelesen wird `/opt/liltress-workstation/vpn/bypass.cidrs`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass reload

Lädt die Präfixe außerhalb des Tunnels neu.

```bash
sudo ./liltress vpn bypass reload
```

Umgehungsrouten werden erneut gesetzt, wenn der Killswitch scharf ist.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn refresh-endpoint <nom>

Löst die Adresse des VPN-Servers erneut auf.

```bash
sudo ./liltress vpn refresh-endpoint <nom>
```

Der DNS-Name des Profils wird erneut aufgelöst.<br/>
Die Adresse ersetzt den gespeicherten Endpunkt.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/service/vpn/cli.sh`

Marken, Killswitch und Startprofil stehen in [vpn.md](vpn.md).

## 📋 Prüfungen

Diese Befehle prüfen. Sie bauen keine Basis neu.

## ▪️ verify alerts

Prüft die Alarmkette und sendet eine Sonde.

```bash
sudo ./liltress verify alerts
```

Die Prüfung kontrolliert den Start-Timer, den periodischen Timer, ClamAV beim Zugriff und den Mailweg.<br/>
Eine Testnachricht geht ab, wenn Alarme aktiv sind. Der Betreff endet mit `Contrôle`.<br/>
`-h` und `--help` zeigen die Nutzung. Jede andere Option wird abgelehnt.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Résultat : OK`

Quelle : `src/workstation/script/verify/alerts.sh`

## ▪️ verify alerts --no-send

Dieselbe Alarmprüfung, ohne Sonde.

```bash
sudo ./liltress verify alerts --no-send
```

Gleicher Umfang wie `verify/alerts.sh`.<br/>
Es wird keine Testnachricht gesendet.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Résultat : OK`

Quelle : `src/workstation/script/verify/alerts.sh`

## ▪️ verify workstation

Prüft den Arbeitsplatz nach der Installation.

```bash
sudo ./liltress verify workstation
```

Das erste Argument ist das Quellverzeichnis.<br/>
Die Schlusszeilen sind `Check Success : N`, `Check Warnings : N` und `Check Fails : N`.<br/>
Die Prüfung kontrolliert auch die SSH-Weiterleitung, die SSH-Firewallregel, USBGuard, das sudo-Zeitlimit und `w-reboot-notice.timer`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Check Success : N`

Quelle : `src/workstation/script/verify/workstation.sh`

## ▪️ verify boot-check

Prüft die Sicherheitsdienste dieses Starts.

```bash
sudo ./liltress verify boot-check
```

Eine noch laufende Prüfung zeigt `Contrôle de démarrage : en cours` und endet mit `1`.<br/>
Eine Abweichung zeigt `Contrôle de démarrage : écarts` und endet mit `1`.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Contrôle de démarrage : OK`

Quelle : `src/workstation/script/boot-check.sh`

## ▪️ verify boot

Startet `boot-check.sh` aus den Quellen.

```bash
sudo ./liltress verify boot
```

`script/verify/boot.sh` führt `script/boot-check.sh` desselben Baums aus.<br/>
Das Ergebnis ist das von `boot-check.sh`.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Erfolg : `Contrôle de démarrage : OK`

Quelle : `src/workstation/script/verify/boot.sh`

## ▪️ verify chkrootkit

Liest einen chkrootkit-Lauf für einen Menschen.

```bash
sudo ./liltress verify chkrootkit
```

Das Journal ist `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`.<br/>
Dieser Befehl schreibt nichts in die Ignorierliste.

Starten Sie ihn aus `/opt/liltress-workstation` oder aus dem Klon.

Quelle : `src/workstation/script/verify/chkrootkit-fp.sh`

## 📋 Wartung

Diese Befehle bauen eine Basis neu, schreiben eine Ignorierliste, oder legen eine Datei zurück.

## ▪️ maintain aide

Baut die AIDE-Datenbank neu.

```bash
sudo ./liltress maintain aide
```

Der Befehl führt `aide --init` aus und ersetzt `/var/lib/aide/aide.db`.<br/>
Die Rechnung kann mehrere Minuten dauern.<br/>
Das ist der Befehl, den die AIDE-Mail beilegt.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Base de référence AIDE enregistrée.`

Quelle : `src/workstation/service/aide/refresh-db.sh`

Installiert : `/opt/liltress-workstation/service/aide/refresh-db.sh`

## ▪️ maintain rkhunter

Baut die rkhunter-Eigenschaftsdatenbank neu.

```bash
sudo ./liltress maintain rkhunter
```

Der Befehl führt `rkhunter --propupd --nocolors` aus.<br/>
Das ist der Befehl der rkhunter-Warnmail.<br/>
Die Mail `mirrors.dat modifié hors rkhunter --update` legt ihn nicht bei.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Base de propriétés rkhunter enregistrée.`

Quelle : `src/share/service/rkhunter/refresh-db.sh`

Installiert : `/opt/liltress-workstation/service/rkhunter/refresh-db.sh`

## ▪️ maintain chkrootkit <journal>

Übernimmt Zeilen eines chkrootkit-Journals als ignoriert.

```bash
sudo ./liltress maintain chkrootkit <journal>
```

Die Datei muss ein `chkrootkit-DATE.log` bereits unter `cron/security_logs/` sein.<br/>
Neue Zeilen werden an `/opt/liltress-workstation/cron/chkrootkit.local.ignore` angehängt.<br/>
Gibt es nichts Neues, lautet die Zeile `Aucune nouvelle détection à enregistrer.` Sonst nennt sie die Zahl in dieser Datei.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Aucune nouvelle détection à enregistrer.`

Erfolg : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`

Quelle : `src/workstation/service/chkrootkit/ignore-log.sh`

Installiert : `/opt/liltress-workstation/service/chkrootkit/ignore-log.sh`

## ▪️ maintain debsums <journal>

Übernimmt Zeilen eines debsums-Journals als ignoriert.

```bash
sudo ./liltress maintain debsums <journal>
```

Die Datei muss ein `debsums-DATE.log` unter `cron/security_logs/` sein.<br/>
Die Muster gehen nach `/opt/liltress-workstation/cron/debsums.local.ignore`.<br/>
Die Erfolgszeilen sind dieselben wie bei chkrootkit. Die debsums-Mail bietet diesen Befehl nicht an: die Prüfung installiert jedes weiter gemeldete Paket bereits neu.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Aucune nouvelle détection à enregistrer.`

Erfolg : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/debsums.local.ignore`

Quelle : `src/workstation/service/debsums/ignore-log.sh`

Installiert : `/opt/liltress-workstation/service/debsums/ignore-log.sh`

## ▪️ maintain scan-skip list

Listet Pfade, die vom ClamAV-Zugriff ausgenommen sind.

```bash
sudo ./liltress maintain scan-skip list
```

Ein akzeptierter Pfad je Zeile.<br/>
Ein abgelehnter Pfad geht auf stderr: `Chemin refusé : <path>`.<br/>
Die Datei ist `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`.

Er kann aus jedem Verzeichnis der Maschine starten.

Quelle : `src/workstation/script/scan-skip.sh`

Installiert : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip add <chemin>

Fügt einen vom ClamAV-Zugriff ausgenommenen Pfad hinzu.

```bash
sudo ./liltress maintain scan-skip add <chemin>
```

Schon vorhanden: `Ce chemin est déjà exclu : <path>`.<br/>
Eine Ablehnung lässt die Datei unverändert und zeigt `Chemin refusé : <path>`.<br/>
Ein Pfad wird unter dem Home des Arbeitsplatzkontos akzeptiert, und darunter.

`/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, das ganze Home, Downloads, Desktop und ihr Elternordner werden abgelehnt.<br/>
Nach einer Änderung schreibt der Befehl den Ausschlussblock von `/etc/clamav/clamd.conf` neu, startet `clamav-daemon` neu, dann `clamav-clamonacc.service` oder `clamonacc.service`.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Chemin ajouté : <path>`

Quelle : `src/workstation/script/scan-skip.sh`

Installiert : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip del <chemin>

Entfernt einen Pfad aus der ClamAV-Ausschlussliste.

```bash
sudo ./liltress maintain scan-skip del <chemin>
```

Fehlt er, zeigt der Befehl `Ce chemin n'est pas dans la liste : <path>` und endet mit `1`.<br/>
Dieselbe Neufassung von `clamd.conf` folgt auf einen erfolgreichen Entzug.

Er kann aus jedem Verzeichnis der Maschine starten.

Erfolg : `Chemin retiré : <path>`

Quelle : `src/workstation/script/scan-skip.sh`

Installiert : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain mirrors save

Speichert den Fingerabdruck der rkhunter-Spiegel.

```bash
sudo ./liltress maintain mirrors save
```

Der SHA-256 von `/var/lib/rkhunter/db/mirrors.dat` wird nach `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256` geschrieben, Modus `600`.<br/>
`refresh-rkhunter-db.sh` tut das nur, nachdem `rkhunter --update` `0` oder `2` zurückgibt.

Er kann aus jedem Verzeichnis der Maschine starten.

Quelle : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Installiert : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain mirrors check

Vergleicht den aktuellen rkhunter-Fingerabdruck mit dem gespeicherten.

```bash
sudo ./liltress maintain mirrors check
```

Es wird nichts gedruckt.<br/>
Exit `0` bedeutet, dass die Fingerabdrücke gleich sind, oder dass noch keiner gespeichert ist.<br/>
Eine Abweichung endet mit einem anderen Code als `0`.

Er kann aus jedem Verzeichnis der Maschine starten.

Quelle : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Installiert : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain restore <id>

Legt eine Quarantänedatei an ihren ursprünglichen Ort zurück.

```bash
sudo ./liltress maintain restore <id>
```

Die Kennung ist eine Zeile von `cron/quarantine.registry`.<br/>
Das Skript ist `core/quarantine/restore.sh`.

Er kann aus jedem Verzeichnis der Maschine starten.

Quelle : `src/share/core/quarantine/restore.sh`

Installiert : `/opt/liltress-workstation/core/quarantine/restore.sh`

## ▪️ maintain mail

Sendet eine Testmail.

```bash
sudo ./liltress maintain mail
```

Die Nachricht dient dazu, Breite und Höhe zu sehen.<br/>
Der Empfänger bleibt der der installierten Konfiguration.<br/>
Das Skript ist `core/mail/mail-test.sh`.

Er kann aus jedem Verzeichnis der Maschine starten.

Quelle : `src/share/core/mail/mail-test.sh`

Installiert : `/opt/liltress-workstation/core/mail/mail-test.sh`
