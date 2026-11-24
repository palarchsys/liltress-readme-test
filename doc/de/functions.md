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

Jeder Befehl unten ist ein echter `./liltress`-Eintrag. Starten Sie ihn aus `/opt/liltress-workstation`, sofern die Zeile nichts anderes sagt.<br/>
Der Erfolgtext ist die Zeile, die das Skript druckt. Er wird nicht übersetzt.

## 📋 liltress

| Befehl | Wirkung | Erfolg oder Exit |
| --- | --- | --- |
| `./liltress help` | Listet die erkannten Befehle: `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`. `-h` und `--help` zeigen dieselbe Hilfe. | Ein unbekannter Befehl druckt die Zeile für den unbekannten Befehl und endet mit `2`. |
| `sudo ./liltress configure` | Stellt die Fragen, schreibt `global.conf` Modus `644` und `secrets.conf` Modus `600`, startet dann `script/run.sh`. | Mailtest: `Essai d'envoi accepté.` Mail aus: `Alertes e-mail coupées. Les contrôles partiront sans envoi.` |
| `sudo ./liltress check` | Prüft `global.conf` und `secrets.conf` ohne Neuschreiben. Gleich `script/configure.sh --check`. | `Configuration conforme` |
| `sudo ./liltress run` | Startet die Installation aus den vorbereiteten Dateien. Verweigert den Start, wenn die Prüfung scheitert. Bereinigt, installiert Pakete, härtet, führt die Sicherheitsprüfungen aus, baut die Datenbanken von AIDE und rkhunter und ruft danach `verify workstation`. | Der Installer stoppt, wenn die Prüfung scheitert. |
| `sudo ./liltress lynis` | Führt `lynis audit system --quick --no-colors` aus. Schreibt den Bericht unter `<source>/log/lynis-<date>/`. | Die letzte Zeile beginnt mit `Rapport :` |
| `sudo ./liltress logs` | Kopiert einen Schnappschuss nach `<source>/log/snapshot-<date>-<host>`. Private Dateien (`secrets.conf`, Schlüssel, Passwörter) werden nicht kopiert. Eine Datei über 8 MiB wird gekürzt, der Schnitt steht in `snapshot/truncated.txt`. | Das Schnappschussverzeichnis ist das Ergebnis. |

## 📋 vpn

| Befehl | Wirkung |
| --- | --- |
| `sudo ./liltress vpn status` | Zeigt Modus `on` oder `off`, ob der Killswitch scharf ist, und das aktive Profil. |
| `sudo ./liltress vpn list` | Listet Profile: Name, Protokoll, Gültigkeit, Validierungsdatum, beobachtetes Datum, letzter Versuch. |
| `sudo ./liltress vpn import wireguard <brand> <name> <directory or file>` | Kopiert das Profil nach `/opt/liltress-workstation/vpn/wireguard/<name>/` und prüft es. Das erste gültige Profil schärft den Modus. |
| `sudo ./liltress vpn import openvpn <brand> <name> <directory or file>` | Kopiert die genannten Dateien. Ein `auth-user-pass` ohne Pfad erhält `auth.txt`. Der Tunnel öffnet nicht, wenn die Zugangsdatei weniger als zwei Zeilen hat. |
| `sudo ./liltress vpn test <name>` | Prüft ein Profil erneut. Ein Fehlschlag entfernt das Validierungsdatum nicht. |
| `sudo ./liltress vpn switch <name>` | Wechselt zu einem anderen noch gültigen Profil. Fährt kein Profil herunter. Startet abhängige Dienste neu, darunter Unbound. |
| `sudo ./liltress vpn mode on` | Schärft den VPN-Modus. Tunnel und Killswitch bleiben bis zum manuellen Stopp. SSH geht nicht in den Tunnel. |
| `sudo ./liltress vpn mode off` | Stoppt den VPN-Modus. Das Netz wird direkt, der Killswitch wird entfernt. Ein späterer erfolgreicher Test schärft den Modus nicht erneut. |
| `sudo ./liltress vpn rm <name>` | Entfernt ein Profil. Das aktive Profil kann nicht entfernt werden. Löscht das Verzeichnis und die Registerzeile. |
| `sudo ./liltress vpn bypass` | Zeigt Präfixe außerhalb des Tunnels. Die Datei ist `/opt/liltress-workstation/vpn/bypass.cidrs`. |
| `sudo ./liltress vpn bypass reload` | Lädt diese Präfixe neu. Umgehungsrouten werden erneut gesetzt, wenn der Killswitch scharf ist. |
| `sudo ./liltress vpn refresh-endpoint <name>` | Löst den DNS-Namen erneut auf und ersetzt den gespeicherten Endpunkt. |
| `sudo ./liltress vpn default` | Zeigt das Startprofil oder keines. |
| `sudo ./liltress vpn default <name>` | Wählt ein gültiges Profil als Start-VPN. Die laufende Sitzung bleibt. |
| `sudo ./liltress vpn default off` | Löscht diese Wahl. Ein offener Tunnel bleibt oben. |

Importregeln, Marken und Killswitch stehen in [vpn.md](vpn.md).

## 📋 verify

| Befehl | Wirkung | Erfolg oder Exit |
| --- | --- | --- |
| `sudo ./liltress verify alerts` | Prüft Start-Timer, periodischen Timer, ClamAV beim Zugriff und den Mailweg. Sendet eine Sonde, wenn Alarme aktiv sind. Der Betreff endet mit `Contrôle`. | `Résultat : OK` |
| `sudo ./liltress verify alerts --no-send` | Dieselbe Prüfung. Keine Sonde. | `Résultat : OK` |
| `sudo ./liltress verify workstation` | Prüfung nach der Installation. Prüft auch SSH-Weiterleitung, die SSH-Firewallregel, USBGuard, das sudo-Zeitlimit und `w-reboot-notice.timer`. | `Check Success : N`, `Check Warnings : N`, `Check Fails : N` |
| `sudo ./liltress verify boot` | Startet `script/boot-check.sh` aus den Quellen. | `Contrôle de démarrage : OK` |
| `sudo ./liltress verify boot-check` | Dieselbe Startprüfung aus jedem Verzeichnis der Zielmaschine. | Noch laufend: `Contrôle de démarrage : en cours`, Exit `1`. Abweichung: `Contrôle de démarrage : écarts`, Exit `1`. |
| `sudo ./liltress verify chkrootkit` | Liest einen chkrootkit-Lauf für einen Menschen. Journal: `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`. | Schreibt die Ignorierliste nicht. |

## 📋 maintain

| Befehl | Wirkung | Erfolgszeile |
| --- | --- | --- |
| `sudo ./liltress maintain aide` | Führt `aide --init` aus und ersetzt `/var/lib/aide/aide.db`. Kann mehrere Minuten dauern. | `Base de référence AIDE enregistrée.` |
| `sudo ./liltress maintain rkhunter` | Führt `rkhunter --propupd --nocolors` aus. | `Base de propriétés rkhunter enregistrée.` |
| `sudo ./liltress maintain chkrootkit <journal>` | Übernimmt neue Zeilen aus einem `chkrootkit-DATE.log` unter `cron/security_logs/`. | `Aucune nouvelle détection à enregistrer.` oder `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore` |
| `sudo ./liltress maintain debsums <journal>` | Übernimmt Zeilen aus einem `debsums-DATE.log` nach `/opt/liltress-workstation/cron/debsums.local.ignore`. Die debsums-Mail bietet diesen Befehl nicht an: die Prüfung installiert jedes weiter gemeldete Paket bereits neu. | Dieselben zwei Formen, mit `debsums.local.ignore`. |
| `sudo ./liltress maintain scan-skip list` | Listet Pfade, die vom ClamAV-Zugriff ausgenommen sind. Datei: `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`. | Ein akzeptierter Pfad je Zeile. Abgelehnt auf stderr: `Chemin refusé : <path>`. |
| `sudo ./liltress maintain scan-skip add <path>` | Fügt eine Ausnahme hinzu, schreibt den Block in `/etc/clamav/clamd.conf` neu, startet `clamav-daemon` neu, dann `clamav-clamonacc.service` oder `clamonacc.service`. | `Chemin ajouté : <path>` |
| `sudo ./liltress maintain scan-skip del <path>` | Entfernt eine Ausnahme und schreibt `clamd.conf` ebenso neu. | `Chemin retiré : <path>` |
| `sudo ./liltress maintain mirrors save` | Schreibt den SHA-256 von `/var/lib/rkhunter/db/mirrors.dat` nach `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, Modus `600`. | Kein festes Banner. Die Datei ist das Ergebnis. |
| `sudo ./liltress maintain mirrors check` | Vergleicht den aktuellen Fingerabdruck mit dem gespeicherten. Druckt nichts. | Exit `0`, wenn gleich oder noch keiner gespeichert ist. Jeder andere Exit bedeutet eine Abweichung. |
| `sudo ./liltress maintain restore <id>` | Legt eine Quarantänedatei zurück. Die Kennung ist eine Zeile von `cron/quarantine.registry`. Skript: `core/quarantine/restore.sh`. | `fichier remis en place` |
| `sudo ./liltress maintain mail` | Sendet einen Layout-Test an den konfigurierten Empfänger. Skript: `core/mail/mail-test.sh`. | Die Testnachricht ist das Ergebnis. |

## ▪️ Grenzen von scan-skip

Ein Pfad wird unter dem Home des Arbeitsplatzkontos akzeptiert, und unterhalb dieses Homes.<br/>
`/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, das ganze Home, Downloads, Desktop und ihr Elternordner werden abgelehnt.<br/>
Schon vorhanden: `Ce chemin est déjà exclu : <path>`.<br/>
Beim Entfernen fehlend: `Ce chemin n'est pas dans la liste : <path>`, Exit `1`.<br/>
Ein abgelehnter Pfad lässt die Datei unverändert.
