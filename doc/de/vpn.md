# VPN

[English](../en/vpn.md) · [Français](../fr/vpn.md) · **Deutsch** · [Español](../es/vpn.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Konfiguration](configure.md)
- [Software und Dienste](software.md)
- [Verfügbare Funktionen](functions.md)
- [Alarm-E-Mails](alerts.md)
- [Befehle nach der Installation](commands.md)
- [Überwachte Bereiche](zones.md)
- **VPN**

Die Installation des Arbeitsplatzes startet ohne VPN. Das Netz bleibt direkt.

## Befehle

```bash
sudo ./liltress vpn import openvpn <brand> <name> <directory or file>
sudo ./liltress vpn import wireguard <brand> <name> <directory or file>
```

Die Marke ist Pflicht. Eine Datei ohne Marke wird abgelehnt.

Das erste gültige Profil wird das Start-VPN und schärft den Killswitch.

Ein ungültiges Bündel wird nicht hinzugefügt. Ist es das erste, bleibt der Killswitch aus.

Ein späterer Import bei ausgeschaltetem Modus ersetzt das Start-VPN nicht und schärft den Killswitch nicht erneut.

Beim Start schärft ein gültiges Start-VPN den Killswitch und öffnet dann dieses Profil.

```bash
sudo ./liltress vpn default
sudo ./liltress vpn default <name>
sudo ./liltress vpn default off
```

`vpn default` zeigt dieses Profil oder keines.

`vpn default <name>` wählt es, wenn es gültig ist. Die laufende Sitzung bleibt.

`vpn default off` löscht diese Wahl. Ein offener Tunnel bleibt oben.

```bash
sudo ./liltress vpn mode on
sudo ./liltress vpn mode off
```

`vpn mode on` schärft den Killswitch und öffnet ein gültiges Profil. Ohne gültiges Profil wird der Befehl abgelehnt und der Killswitch bleibt aus.

`vpn mode off` entschärft den Killswitch und kehrt zum direkten Netz zurück. Das Start-VPN bleibt für den nächsten Start. Die Profilverzeichnisse bleiben.

Das Profil erscheint unter Einstellungen, Netzwerk. Der Schalter ist der Modus. Der Import ist nicht die Schaltfläche `+`.

```bash
sudo ./liltress vpn switch <name>
```

Wechselt zu einem anderen gültigen Profil. Der Killswitch bleibt während des Wechsels scharf. Das Start-VPN ändert sich nicht.

Ein fehlgeschlagener Wechsel stellt das direkte Internet nicht wieder her und entfernt kein Profil.

Ein Profil, das bereits geantwortet hat und dann drei Versuche von zehn Sekunden scheitert, wird entfernt.

Nimmt diese Entfernung das letzte Profil, wird das Netz direkt und der Killswitch entschärft. Die Alarmmail geht vor dieser Entschärfung hinaus.

```bash
sudo ./liltress vpn rm <name>
sudo ./liltress vpn test <name>
sudo ./liltress vpn list
sudo ./liltress vpn status
```

`vpn rm` entfernt ein Profil. Das Entfernen des Start-VPN probiert das nächste Profil der Liste, nach einer Prüfung. Das Entfernen des letzten löscht das Start-VPN und entschärft den Killswitch.

Ein aktives Profil, das nicht das Start-VPN ist, bleibt, solange ein anderes Profil existiert.

Ein fehlgeschlagener `vpn test` lässt das Verzeichnis.

Jedes WireGuard-Profil ist `/opt/liltress-workstation/vpn/wireguard/<name>/`.

Jedes OpenVPN-Profil ist `/opt/liltress-workstation/vpn/openvpn/<name>/`.

Die Datei `meta` in diesem Verzeichnis hält die Marke fest.

Das Register ist `/opt/liltress-workstation/vpn/vpn.json`.

`vpn list` zeigt den Namen, das Protokoll, die Marke und die Daten.

SSH bleibt außerhalb des Tunnels.

Die Bypass-Datei enthält ein Präfix je Zeile oder eine Klammerzeile. Das Beispiel ist `[192.168.1.0/24, 200.200.200.0/24]`.

```bash
sudo ./liltress vpn bypass
sudo ./liltress vpn bypass reload
sudo ./liltress vpn refresh-endpoint <name>
```

Unbound wird nach einem Moduswechsel oder einem Wechsel neu gestartet.

Globales IPv6 wird gesperrt, wenn der Tunnel kein IPv6 hat.

Ein öffentlicher Bereich in der Bypass-Datei geht im Klartext hinaus.

Nachdem das letzte Profil entfernt wurde, wird das nächste gültige Profil wieder das Start-VPN und schärft den Killswitch.

## Importformen

Das Verzeichnis oder die Datei enthält genau die Dateinamen der Marke.

Eine zusätzliche Datei, eine fehlende Datei oder die Datei des anderen Protokolls wird abgelehnt.

Ein Verzeichnis wird geöffnet. `auth.txt` darf neben diesem Verzeichnis liegen.

Das Tagesjournal ist `log/vpn/YYYY-MM-DD.log`.

Steht das Wort `REPLACE` noch in der Datei, stoppt der Import vor dem Handshake.

Die Beispieladresse `203.0.113.10` kann keinen Tunnel öffnen.

`auth.txt` hat zwei Zeilen: den Namen, dann das Geheimnis.

Ein Zertifikat, das in der Datei steht, bleibt im Profil.

Ein neben der Datei genanntes Zertifikat wird unter diesem Namen kopiert.

`up`- und `down`-Skripte des Anbieters werden nicht kopiert.

Der DNS des Anbieters wird nicht benutzt. Unbound bleibt der Resolver.

Das erste `remote` ist der Server, der geöffnet wird.

## Marken

| Marke | Angenommen | Abgelehnt | Beispielbefehl |
| --- | --- | --- | --- |
| NordVPN | OpenVPN, Zertifikate innerhalb der `.ovpn`. `auth.txt` ist der Dienstbenutzername, dann das Dienstpasswort. | Konto-E-Mail. WireGuard. NordLynx. | `sudo ./liltress vpn import openvpn nordvpn nord-fr /chemin/nordvpn` |
| ExpressVPN | Eine `.ovpn` von der Seite der manuellen Konfiguration. `auth.txt` ist Benutzername und Passwort dieser Seite. | WireGuard. Lightway. | `sudo ./liltress vpn import openvpn expressvpn express-fr /chemin/expressvpn` |
| Surfshark | OpenVPN mit `config.ovpn` und `auth.txt`, oder nur WireGuard `config.conf`. | Die Datei des anderen Protokolls. Die E-Mail der Seite. | `sudo ./liltress vpn import openvpn surfshark surf-fr /chemin/surfshark-openvpn` |
| Proton VPN | OpenVPN Linux `config.ovpn` plus OpenVPN-Benutzername und Geheimnis, oder nur WireGuard `config.conf`. | Das Postfachpasswort. Die Datei des anderen Protokolls. | `sudo ./liltress vpn import wireguard protonvpn proton-fr /chemin/protonvpn-wireguard.conf` |
| CyberGhost | `openvpn.ovpn`, `ca.crt`, `client.crt`, `client.key`, `auth.txt`. | WireGuard. Ein fehlendes Zertifikat. Die Kontoanmeldung. | `sudo ./liltress vpn import openvpn cyberghost cyberghost-fr /chemin/cyberghost` |
| Private Internet Access | `config.ovpn` und `auth.txt`. Der Benutzername ist der Name `p` gefolgt von den Ziffern. | WireGuard. Ein SOCKS-Benutzername. | `sudo ./liltress vpn import openvpn pia pia-fr /chemin/pia` |
| Mullvad | WireGuard-Datei, kopiert als `config.conf`. Kein `auth.txt`. | OpenVPN. | `sudo ./liltress vpn import wireguard mullvad mullvad-se /chemin/mullvad.conf` |
| PureVPN | OpenVPN mit genannten `ca`, `cert`, `key`, plus VPN-Benutzername und Geheimnis. Oder WireGuard `config.conf`. | Die E-Mail. Die Datei des anderen Protokolls. Eine WireGuard-Datei, die nicht innerhalb von dreißig Minuten aktiviert wird, scheitert am Handshake und wird nicht veröffentlicht. | `sudo ./liltress vpn import openvpn purevpn pure-fr /chemin/purevpn-openvpn` |
| IPVanish | `config.ovpn` und `auth.txt`. Zertifikate in der Datei oder daneben, wenn sie genannt sind. | WireGuard. | `sudo ./liltress vpn import openvpn ipvanish ipvanish-fr /chemin/ipvanish` |
| Windscribe | OpenVPN `config.ovpn` und Get Credentials `auth.txt`, oder WireGuard `config.conf`. | Die Anmeldung der Website. IKEv2. | `sudo ./liltress vpn import openvpn windscribe windscribe-fr /chemin/windscribe-openvpn` |
| Perso | WireGuard-Datei von der wg-easy-Seite des Servers, kopiert als `config.conf`. | OpenVPN. Ein Archiv. | `sudo ./liltress vpn import wireguard perso poste-1 /chemin/perso.conf` |

Ersetzen Sie `REPLACE` in jeder Datei des Verzeichnisses oder in der WireGuard-Datei vor dem Import.

Surfshark, Proton VPN, PureVPN und Windscribe nehmen beide Protokolle an, aber nicht beide Dateien in einem Import.

Der Beispielendpunkt Perso ist `203.0.113.10:51820`. Die Beispieladresse ist `10.8.0.2/24`.
