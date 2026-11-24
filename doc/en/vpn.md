<div align="center">

# 🛡️ VPN 🛡️

</div>

**English** · [Français](../fr/vpn.md) · [Deutsch](../de/vpn.md) · [Español](../es/vpn.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Software and services](software.md)
- [Available functions](functions.md)
- [Alert emails](alerts.md)
- [Commands after installation](commands.md)
- [Monitored zones](zones.md)
- **VPN**

---

The workstation install starts with no VPN. The network stays direct.

## 📋 Commands

```bash
sudo ./liltress vpn import openvpn <brand> <name> <directory or file>
sudo ./liltress vpn import wireguard <brand> <name> <directory or file>
```

The brand is required. A file without a brand is refused.<br/>
The first valid profile becomes the startup VPN and arms the killswitch.<br/>
An invalid bundle is not added. When it is the first, the killswitch stays off.<br/>
A later import, while mode is off, does not replace the startup VPN and does not arm the killswitch again.<br/>
At startup, a valid startup VPN arms the killswitch, then opens that profile.

```bash
sudo ./liltress vpn default
sudo ./liltress vpn default <name>
sudo ./liltress vpn default off
```

`vpn default` shows that profile, or none.<br/>
`vpn default <name>` selects it when it is valid. The current session stays as it is.<br/>
`vpn default off` clears that choice. An open tunnel stays up.

```bash
sudo ./liltress vpn mode on
sudo ./liltress vpn mode off
```

`vpn mode on` arms the killswitch and opens a valid profile. With no valid profile, the command is refused and the killswitch stays off.<br/>
`vpn mode off` disarms the killswitch and returns to the direct network. The startup VPN stays for the next boot. The profile directories stay.<br/>
The profile appears in Settings, Network. The toggle is the mode. Import is not the `+` button.

```bash
sudo ./liltress vpn switch <name>
```

Moves to another valid profile. The killswitch stays armed during the switch. The startup VPN does not change.<br/>
A failed switch does not restore direct Internet and does not remove a profile.<br/>
A profile that has already answered, then fails three tries of ten seconds, is removed.<br/>
When that removal takes the last profile, the network goes direct and the killswitch is disarmed. The alert mail is sent before that disarm.

```bash
sudo ./liltress vpn rm <name>
sudo ./liltress vpn test <name>
sudo ./liltress vpn list
sudo ./liltress vpn status
```

`vpn rm` removes a profile. Removing the startup VPN tries the next profile in the list, after a check. Removing the last one clears the startup VPN and disarms the killswitch.<br/>
An active profile that is not the startup VPN stays while another profile exists.<br/>
A failed `vpn test` leaves the directory.<br/>
Each WireGuard profile is `/opt/liltress-workstation/vpn/wireguard/<name>/`.<br/>
Each OpenVPN profile is `/opt/liltress-workstation/vpn/openvpn/<name>/`.<br/>
The `meta` file in that directory records the brand.<br/>
The registry is `/opt/liltress-workstation/vpn/vpn.json`.<br/>
`vpn list` shows the name, the protocol, the brand, and the dates.<br/>
SSH stays off the tunnel.<br/>
The bypass file is one prefix per line, or one bracket line. The example is `[192.168.1.0/24, 200.200.200.0/24]`.

```bash
sudo ./liltress vpn bypass
sudo ./liltress vpn bypass reload
sudo ./liltress vpn refresh-endpoint <name>
```

Unbound is restarted after a mode change or a switch.<br/>
Global IPv6 is blocked when the tunnel has no IPv6.<br/>
A public range written in the bypass file leaves in the clear.<br/>
After the last profile is removed, the next valid profile becomes the startup VPN again and arms the killswitch.

## 📋 Import forms

The directory or the file holds exactly the brand file names.<br/>
An extra file, a missing file, or the other protocol file is refused.<br/>
One directory is opened. `auth.txt` may sit beside that directory.<br/>
The day journal is `log/vpn/YYYY-MM-DD.log`.<br/>
If the word `REPLACE` is still present, the import stops before the handshake.<br/>
The example address `203.0.113.10` cannot open a tunnel.<br/>
`auth.txt` has two lines: the name, then the secret.<br/>
A certificate written inside the file stays in the profile.<br/>
A certificate named beside the file is copied under that name.<br/>
Provider `up` and `down` scripts are not copied.<br/>
Provider DNS is not used. Unbound stays the resolver.<br/>
The first `remote` is the server that is opened.

## 📋 Brands

| Brand | Accepted | Refused | Example command |
| --- | --- | --- | --- |
| NordVPN | OpenVPN, certificates inside the `.ovpn`. `auth.txt` is the service username then the service password. | Account email. WireGuard. NordLynx. | `sudo ./liltress vpn import openvpn nordvpn nord-fr /chemin/nordvpn` |
| ExpressVPN | One `.ovpn` from the manual configuration page. `auth.txt` is the username and password shown on that page. | WireGuard. Lightway. | `sudo ./liltress vpn import openvpn expressvpn express-fr /chemin/expressvpn` |
| Surfshark | OpenVPN with `config.ovpn` and `auth.txt`, or WireGuard `config.conf` only. | The other protocol file. The site email. | `sudo ./liltress vpn import openvpn surfshark surf-fr /chemin/surfshark-openvpn` |
| Proton VPN | OpenVPN Linux `config.ovpn` plus OpenVPN username and secret, or WireGuard `config.conf` only. | The mailbox password. The other protocol file. | `sudo ./liltress vpn import wireguard protonvpn proton-fr /chemin/protonvpn-wireguard.conf` |
| CyberGhost | `openvpn.ovpn`, `ca.crt`, `client.crt`, `client.key`, `auth.txt`. | WireGuard. A missing certificate. The account login. | `sudo ./liltress vpn import openvpn cyberghost cyberghost-fr /chemin/cyberghost` |
| Private Internet Access | `config.ovpn` and `auth.txt`. The username is the `p` name followed by the digits. | WireGuard. A SOCKS username. | `sudo ./liltress vpn import openvpn pia pia-fr /chemin/pia` |
| Mullvad | WireGuard file copied as `config.conf`. No `auth.txt`. | OpenVPN. | `sudo ./liltress vpn import wireguard mullvad mullvad-se /chemin/mullvad.conf` |
| PureVPN | OpenVPN with named `ca`, `cert`, `key` when cited, plus VPN username and secret. Or WireGuard `config.conf`. | The email. The other protocol file. A WireGuard file not activated within thirty minutes fails the handshake and is not published. | `sudo ./liltress vpn import openvpn purevpn pure-fr /chemin/purevpn-openvpn` |
| IPVanish | `config.ovpn` and `auth.txt`. Certificates inside the file, or beside it when named. | WireGuard. | `sudo ./liltress vpn import openvpn ipvanish ipvanish-fr /chemin/ipvanish` |
| Windscribe | OpenVPN `config.ovpn` and Get Credentials `auth.txt`, or WireGuard `config.conf`. | The website login. IKEv2. | `sudo ./liltress vpn import openvpn windscribe windscribe-fr /chemin/windscribe-openvpn` |
| Perso | WireGuard file from the server wg-easy page, copied as `config.conf`. | OpenVPN. An archive. | `sudo ./liltress vpn import wireguard perso poste-1 /chemin/perso.conf` |

Replace `REPLACE` in every file of the directory, or in the WireGuard file, before import.<br/>
Surfshark, Proton VPN, PureVPN and Windscribe accept both protocols, but not both files in one import.<br/>
The Perso example endpoint is `203.0.113.10:51820`. The example address is `10.8.0.2/24`.
