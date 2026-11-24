<div align="center">

# 🛡️ Befehle nach der Installation 🛡️

</div>

[English](../en/commands.md) · [Français](../fr/commands.md) · **Deutsch** · [Español](../es/commands.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Konfiguration](configure.md)
- [Software und Dienste](software.md)
- [Verfügbare Funktionen](functions.md)
- [Alarm-E-Mails](alerts.md)
- **Befehle nach der Installation**
- [Überwachte Bereiche](zones.md)
- [VPN](vpn.md)

---

Führen Sie diese Befehle aus `/opt/liltress-workstation` aus.

## 📋 Konfiguration prüfen

```bash
sudo ./liltress check
```

Prüft `global.conf` und `secrets.conf`.<br/>
Die Erfolgszeile ist `Configuration conforme`.

## 📋 Installer erneut starten

```bash
sudo ./liltress run
```

Startet aus den vorbereiteten Dateien.<br/>
Der Installer verweigert den Start, wenn die Prüfung nicht besteht.

## 📋 Alarmweg prüfen

```bash
sudo ./liltress verify alerts
```

Prüft den Start-Timer, den periodischen Timer, ClamAV beim Zugriff und den Mailweg.<br/>
Ohne `--no-send` geht eine Sonde ab, wenn Alarme aktiv sind.<br/>
Der Betreff endet mit `Contrôle`.<br/>
Die Erfolgszeile ist `Résultat : OK`.

```bash
sudo ./liltress verify alerts --no-send
```

Dieselbe Prüfung. Keine Sonde.

## 📋 Arbeitsplatz prüfen

```bash
sudo ./liltress verify workstation
```

Schlusszeilen: `Check Success : N`, `Check Warnings : N`, `Check Fails : N`.<br/>
Dieselbe Prüfung kontrolliert SSH-Weiterleitung, die SSH-Firewallregel, USBGuard, das sudo-Zeitlimit und `w-reboot-notice.timer`.

## 📋 Diesen Start prüfen

```bash
sudo ./liltress verify boot
```

Erfolg: `Contrôle de démarrage : OK`.<br/>
Noch laufend: `Contrôle de démarrage : en cours`, Exit `1`.<br/>
Abweichung: `Contrôle de démarrage : écarts`, Exit `1`.

## 📋 Lynis

```bash
sudo ./liltress lynis
```

Führt `lynis audit system --quick --no-colors` aus.<br/>
Der Bericht liegt unter `<source>/log/lynis-<stamp>/`.<br/>
`LYNIS_MIN_SCORE` ist 80.

## 📋 Basen neu aufbauen

```bash
sudo ./liltress maintain aide
sudo ./liltress maintain rkhunter
```

AIDE-Erfolg: `Base de référence AIDE enregistrée.`<br/>
rkhunter-Erfolg: `Base de propriétés rkhunter enregistrée.`

## 📋 rkhunter-Spiegelstempel

```bash
sudo ./liltress maintain mirrors save
sudo ./liltress maintain mirrors check
```

`mirrors save` schreibt den SHA-256 nach `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, Modus `600`.<br/>
`mirrors check` schreibt nichts. Exit `0` bedeutet, dass die Fingerabdrücke gleich sind oder noch keiner gespeichert ist.

## 📋 Vom Zugriff ausgenommene Pfade

```bash
sudo ./liltress maintain scan-skip list
sudo ./liltress maintain scan-skip add $HOME/.cache/exemple
sudo ./liltress maintain scan-skip del $HOME/.cache/exemple
```

| Aktion | Gedruckte Zeile |
| --- | --- |
| `list` | ein akzeptierter Pfad je Zeile |
| `add` | `Chemin ajouté : <path>` |
| schon vorhanden | `Ce chemin est déjà exclu : <path>` |
| `del` | `Chemin retiré : <path>` |
| abgelehnt | `Chemin refusé : <path>` |

Die volle Liste, einschließlich VPN und Quarantäne-Wiederherstellung, steht in [functions.md](functions.md).
