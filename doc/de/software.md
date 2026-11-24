<div align="center">

# 🛡️ Software und Dienste 🛡️

</div>

[English](../en/software.md) · [Français](../fr/software.md) · **Deutsch** · [Español](../es/software.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Konfiguration](configure.md)
- **Software und Dienste**
- [Verfügbare Funktionen](functions.md)
- [Alarm-E-Mails](alerts.md)
- [Befehle nach der Installation](commands.md)
- [Überwachte Bereiche](zones.md)
- [VPN](vpn.md)

---

## 📋 Dieser Leitfaden

Die Installation legt die Pakete und Dienste unten an. Jeder wird aus `config/service/<name>/` und dem passenden Skript unter `service/` konfiguriert.

| Name | Rolle | Was der Arbeitsplatz ändert |
| --- | --- | --- |
| openssh-server | SSH-Server | Root-Anmeldung wird abgelehnt. `PasswordAuthentication yes`. `AllowTcpForwarding no`. `AllowAgentForwarding no`. |
| ufw | Host-Firewall | Eingehend ist verweigern. Die SSH-Regel bleibt ausdrücklich. |
| unbound | Lokaler DNS-Resolver | Lauscht auf `127.0.0.1`, Port 53. DNS aus einem VPN-Profil wird nicht benutzt. |
| usbguard, usbguard-notifier | USB-Regeln | Bereits gesteckte Geräte sowie Tastaturen, Mäuse und Hubs sind erlaubt. Ein neuer Speicher wartet auf eine Bestätigung am Desktop. |
| fail2ban | Sperren | Wiederholte Anmeldefehler werden gesperrt. |
| auditd | Audit | Schreibt Regeln für die gewählten Pfade. |
| aide | Dateiintegrität | Datenbank `/var/lib/aide/aide.db`. Neuaufbau: `./liltress maintain aide`. |
| rkhunter | Rootkit und Eigenschaften | Eigenschaftsdatenbank: `./liltress maintain rkhunter`. `/dev/shm` und der Socket `/dev/log` sind erlaubt. |
| chkrootkit | Rootkit-Prüfung | Binärdatei `/usr/sbin/chkrootkit`. Bekannte Störzeilen können aus einem Journal übernommen werden. |
| clamav | Schadsoftware | Zugriffsscan, Startscan und ein stündlicher Scan. Ein Fund kann die Datei in Quarantäne verschieben. |
| crowdsec | Lokale Entscheidungen | Lokale Entscheidungen aus den gelieferten Aufnahmen. |
| debsums | Paketprüfsummen | Ein weiter gemeldetes Paket wird mit `apt-get install --reinstall` neu installiert. |
| wireguard-tools, openvpn, conntrack | Ein Tunnel | Bleibt aus, bis ein Profil gültig ist. SSH geht nicht in den Tunnel. |
| apparmor | Distributionsprofile | `service/apparmor/userns.sh` entfernt ein nicht geliefertes hfu-Profil. |
| unattended-upgrades | Sicherheitsupdates | Sicherheitsquellen. `Automatic-Reboot` ist `false`. |
| postfix | Alarmmail | Nur installiert, wenn `MAIL_ALERTS=1`. |
| lynis | Härtungsaudit | `LYNIS_MIN_SCORE` ist 80. `verify workstation` akzeptiert einen `hardening_index` größer oder gleich diesem Wert. |
| ubuntu-pro-client | Ubuntu-Pro-Anbindung | Bindet den Token. Eine Nicht-LTS aktiviert esm-infra, esm-apps und Livepatch nicht. Auf LTS stoppt ein Fehlschlag dieser Aktivierung die Installation nicht. |
| acct, sysstat, libpam-pwquality | Accounting und Passwörter | Prozessaccounting, Aktivitätsstatistik, Passwortqualität. |
| rng-tools-debian, haveged | Entropie | Zusätzliche Entropiequellen. |
| apt-listchanges, needrestart, apt-show-versions | Pakethinweise | Änderungshinweise und Neustartprüfungen. Die Neustartmail startet die Maschine nicht neu. |

Eine Dienstdatei ist keine zweite Kopie einer Vertragsvariable. Die Werte stehen in `global.conf`, `user.conf` oder `secrets.conf`.
