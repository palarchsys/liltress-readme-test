<div align="center">

# 🛡️ Überwachte Bereiche 🛡️

</div>

[English](../en/zones.md) · [Français](../fr/zones.md) · **Deutsch** · [Español](../es/zones.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Konfiguration](configure.md)
- [Software und Dienste](software.md)
- [Verfügbare Funktionen](functions.md)
- [Alarm-E-Mails](alerts.md)
- [Befehle nach der Installation](commands.md)
- **Überwachte Bereiche**
- [VPN](vpn.md)

---

Die Farben helfen beim Lesen. Sie sind keine zweite Richtlinie. Die Werkzeuge in der Zeile sind die Richtlinie.

| Bereich | Stufe | Werkzeuge | Hinweise |
| --- | --- | --- | --- |
| `/home` ohne die gelieferten Ausnahmen | 🔴 Hoch | ClamAV beim Zugriff, periodisches ClamAV, AIDE | Benutzerdateien. |
| `~/.cache`, `~/snap` | ⚪ Ausgenommen | Ausnahme beim Zugriff | Geliefert in `config/service/clamav/scan-skip.paths`. Der periodische Scan geht weiter über Pfade, die beim Zugriff ausgenommen sind. |
| Downloads, Desktop | 🔴 Hoch | ClamAV beim Zugriff | `scan-skip add` lehnt diese Pfade, ihren Elternordner, `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root` und das ganze Home ab. |
| `/etc` | 🟡 Standard | AIDE, auditd | Konfiguration, von der Integritätsbasis und den Audit-Regeln beobachtet. |
| `/opt/liltress-workstation` | 🟠 Mittel | AIDE, lokale Journale | Produktbaum, Alarmkopien und Sicherheitsjournale. |
| `/var/log` | 🟢 Leicht | Logrotation, auditd | `chkrootkit`-Muster ignorieren `/var/log`. |
| USB-Speicher | 🟡 Standard | usbguard | Ein neuer Speicher wartet auf eine Desktop-Bestätigung. Tastaturen, Mäuse, Hubs und bereits vorhandene Geräte sind erlaubt. |
| Gepackte Dateien unter `/usr` | 🟢 Leicht | debsums, rkhunter | Prüfsummen und Eigenschaftsdatenbank. Eine debsums-Abweichung kann das Paket neu installieren. |

## 📋 Legende

- 🔴 Hoch: Zugriffsscan plus Integrität, oder ein Pfad, der nicht ausgenommen werden kann.
- 🟠 Mittel: Integrität und der Produktbaum.
- 🟡 Standard: Konfiguration, Audit oder eine Gerätebestätigung.
- 🟢 Leicht: Prüfsummen, Eigenschaften oder Journale.
- ⚪ Ausgenommen: in der Zugriffsliste genannt. Das ist kein stilles Loch im periodischen Scan.

Eine Stufe ändern heißt die Werkzeugkonfiguration ändern (`scan-skip`, AIDE, auditd-Watches, usbguard), nicht die Farbe in dieser Tabelle.
