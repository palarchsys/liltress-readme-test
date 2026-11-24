# Installation

[English](../en/install.md) · [Français](../fr/install.md) · **Deutsch** · [Español](../es/install.md)

## Navigation

- [Index](README.md)
- **Installation**
- [Konfiguration](configure.md)
- [Software und Dienste](software.md)
- [Verfügbare Funktionen](functions.md)
- [Alarm-E-Mails](alerts.md)
- [Befehle nach der Installation](commands.md)
- [Überwachte Bereiche](zones.md)
- [VPN](vpn.md)

## Vor dem Start

Drei Punkte stehen vor der Installation: das System, der Zugriff und die externen Angaben.

### System

Ubuntu 26.04 oder neuer, einschließlich einer Zwischenversion.

Eine frische Installation ist vorzuziehen.

Nutzen Sie eine lokale Konsole oder die vorhandene grafische Sitzung.

Die Maschine braucht Internetzugang.

### Zugriff

Sie brauchen `sudo` für `./liltress configure`, `./liltress run` und die Prüfungen.

### Externe Angaben

| Bedarf | Vorgehen |
| --- | --- |
| Ubuntu-Pro-Token | Pflicht. Öffnen Sie [https://ubuntu.com/pro/dashboard](https://ubuntu.com/pro/dashboard) und kopieren Sie den Token. Nur Buchstaben und Ziffern, Länge 6 bis 100, ohne Leerzeichen. |
| Anwendungspasswort | Nur wenn Mail-Alarme aktiv sind. Nutzen Sie das Anwendungspasswort des Anbieters. Niemals das Kontopasswort. Länge 8 bis 128. Leerzeichen werden entfernt. |
| SMTP-Server | Nur wenn Alarme aktiv sind. Form `host:port`. Beispiel: `smtp-mail.outlook.com:587`. Klammern werden abgelehnt. `swaks` sendet eine Testnachricht. |

## Curl

```bash
sudo apt-get install -y ca-certificates curl git swaks >/dev/null 2>&1
curl -fsSL https://raw.githubusercontent.com/palarchsys/liltress-workstation/main/script/install.sh | sudo bash
```

`install.sh` fragt `[O/N]` im Terminal.

`O` ersetzt den installierten Baum durch den geholten Zweig.

`O` startet `script/run.sh`, wenn `script/configure.sh --check` gelingt.

Jede andere Antwort stoppt.

Eine fehlgeschlagene Prüfung startet `script/configure.sh`.

## Clone

```bash
sudo git clone https://github.com/palarchsys/liltress-workstation /opt/liltress-workstation
cd /opt/liltress-workstation
sudo ./liltress configure
sudo bash /opt/liltress-workstation/script/version-update.sh
sudo ./liltress verify boot
```

Spätere Befehle laufen aus `/opt/liltress-workstation`.
