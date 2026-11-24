<div align="center">

# 🛡️ VPN 🛡️

</div>

[English](../en/vpn.md) · [Français](../fr/vpn.md) · [Deutsch](../de/vpn.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- [Configuración](configure.md)
- [Software y servicios](software.md)
- [Funciones disponibles](functions.md)
- [Correos de alerta](alerts.md)
- [Comandos después de la instalación](commands.md)
- [Mapa de zonas vigiladas](zones.md)
- **VPN**

---

La instalación del puesto empieza sin VPN. La red permanece directa.

## 📋 Comandos

```bash
sudo ./liltress vpn import openvpn <brand> <name> <directory or file>
sudo ./liltress vpn import wireguard <brand> <name> <directory or file>
```

La marca es obligatoria. Un archivo sin marca se rechaza.<br/>
El primer perfil válido se convierte en el VPN de arranque y arma el killswitch.<br/>
Un lote inválido no se añade. Si es el primero, el killswitch permanece apagado.<br/>
Una importación posterior, con el modo apagado, no sustituye el VPN de arranque y no vuelve a armar el killswitch.<br/>
Al arrancar, un VPN de arranque válido arma el killswitch y después abre ese perfil.

```bash
sudo ./liltress vpn default
sudo ./liltress vpn default <name>
sudo ./liltress vpn default off
```

`vpn default` muestra ese perfil, o ninguno.<br/>
`vpn default <name>` lo selecciona si es válido. La sesión en curso permanece.<br/>
`vpn default off` borra esa elección. Un túnel abierto permanece arriba.

```bash
sudo ./liltress vpn mode on
sudo ./liltress vpn mode off
```

`vpn mode on` arma el killswitch y abre un perfil válido. Sin perfil válido, el comando se rechaza y el killswitch permanece apagado.<br/>
`vpn mode off` desarma el killswitch y vuelve a la red directa. El VPN de arranque permanece para el siguiente arranque. Los directorios de perfil permanecen.<br/>
El perfil aparece en Configuración, Red. El interruptor es el modo. La importación no es el botón `+`.

```bash
sudo ./liltress vpn switch <name>
```

Pasa a otro perfil válido. El killswitch permanece armado durante el cambio. El VPN de arranque no cambia.<br/>
Un cambio fallido no restaura Internet directo y no retira un perfil.<br/>
Un perfil que ya ha respondido y después falla tres intentos de diez segundos se retira.<br/>
Cuando esa retirada se lleva el último perfil, la red pasa a ser directa y el killswitch se desarma. El correo de alerta se envía antes de ese desarme.

```bash
sudo ./liltress vpn rm <name>
sudo ./liltress vpn test <name>
sudo ./liltress vpn list
sudo ./liltress vpn status
```

`vpn rm` retira un perfil. Retirar el VPN de arranque prueba el siguiente perfil de la lista, después de una comprobación. Retirar el último borra el VPN de arranque y desarma el killswitch.<br/>
Un perfil activo que no es el VPN de arranque permanece mientras exista otro perfil.<br/>
Un `vpn test` fallido deja el directorio.<br/>
Cada perfil WireGuard es `/opt/liltress-workstation/vpn/wireguard/<name>/`.<br/>
Cada perfil OpenVPN es `/opt/liltress-workstation/vpn/openvpn/<name>/`.<br/>
El archivo `meta` de ese directorio registra la marca.<br/>
El registro es `/opt/liltress-workstation/vpn/vpn.json`.<br/>
`vpn list` muestra el nombre, el protocolo, la marca y las fechas.<br/>
SSH permanece fuera del túnel.<br/>
El archivo de desvío es un prefijo por línea, o una línea entre corchetes. El ejemplo es `[192.168.1.0/24, 200.200.200.0/24]`.

```bash
sudo ./liltress vpn bypass
sudo ./liltress vpn bypass reload
sudo ./liltress vpn refresh-endpoint <name>
```

Unbound se reinicia después de un cambio de modo o de un cambio de perfil.<br/>
El IPv6 global se bloquea cuando el túnel no tiene IPv6.<br/>
Un rango público escrito en el archivo de desvío sale en claro.<br/>
Después de retirar el último perfil, el siguiente perfil válido vuelve a ser el VPN de arranque y arma el killswitch.

## 📋 Formas de importación

El directorio o el archivo contiene exactamente los nombres de archivo de la marca.<br/>
Un archivo de más, un archivo que falta o el archivo del otro protocolo se rechaza.<br/>
Se abre un directorio. `auth.txt` puede estar junto a ese directorio.<br/>
El diario del día es `log/vpn/YYYY-MM-DD.log`.<br/>
Si la palabra `REPLACE` sigue presente, la importación se detiene antes del handshake.<br/>
La dirección de ejemplo `203.0.113.10` no puede abrir un túnel.<br/>
`auth.txt` tiene dos líneas: el nombre y después el secreto.<br/>
Un certificado escrito dentro del archivo permanece en el perfil.<br/>
Un certificado nombrado junto al archivo se copia con ese nombre.<br/>
Los scripts `up` y `down` del proveedor no se copian.<br/>
El DNS del proveedor no se usa. Unbound permanece como resolutor.<br/>
El primer `remote` es el servidor que se abre.

## 📋 Marcas

| Marca | Aceptado | Rechazado | Comando de ejemplo |
| --- | --- | --- | --- |
| NordVPN | OpenVPN, certificados dentro del `.ovpn`. `auth.txt` es el nombre de usuario del servicio y después la contraseña del servicio. | Correo de la cuenta. WireGuard. NordLynx. | `sudo ./liltress vpn import openvpn nordvpn nord-fr /chemin/nordvpn` |
| ExpressVPN | Un `.ovpn` de la página de configuración manual. `auth.txt` es el nombre de usuario y la contraseña mostrados en esa página. | WireGuard. Lightway. | `sudo ./liltress vpn import openvpn expressvpn express-fr /chemin/expressvpn` |
| Surfshark | OpenVPN con `config.ovpn` y `auth.txt`, o solo WireGuard `config.conf`. | El archivo del otro protocolo. El correo del sitio. | `sudo ./liltress vpn import openvpn surfshark surf-fr /chemin/surfshark-openvpn` |
| Proton VPN | OpenVPN Linux `config.ovpn` más nombre de usuario y secreto OpenVPN, o solo WireGuard `config.conf`. | La contraseña del buzón. El archivo del otro protocolo. | `sudo ./liltress vpn import wireguard protonvpn proton-fr /chemin/protonvpn-wireguard.conf` |
| CyberGhost | `openvpn.ovpn`, `ca.crt`, `client.crt`, `client.key`, `auth.txt`. | WireGuard. Un certificado que falta. El inicio de sesión de la cuenta. | `sudo ./liltress vpn import openvpn cyberghost cyberghost-fr /chemin/cyberghost` |
| Private Internet Access | `config.ovpn` y `auth.txt`. El nombre de usuario es el nombre `p` seguido de las cifras. | WireGuard. Un nombre de usuario SOCKS. | `sudo ./liltress vpn import openvpn pia pia-fr /chemin/pia` |
| Mullvad | Archivo WireGuard copiado como `config.conf`. Sin `auth.txt`. | OpenVPN. | `sudo ./liltress vpn import wireguard mullvad mullvad-se /chemin/mullvad.conf` |
| PureVPN | OpenVPN con `ca`, `cert`, `key` nombrados cuando se citan, más nombre de usuario y secreto VPN. O WireGuard `config.conf`. | El correo. El archivo del otro protocolo. Un archivo WireGuard no activado en treinta minutos falla el handshake y no se publica. | `sudo ./liltress vpn import openvpn purevpn pure-fr /chemin/purevpn-openvpn` |
| IPVanish | `config.ovpn` y `auth.txt`. Certificados dentro del archivo, o junto a él cuando están nombrados. | WireGuard. | `sudo ./liltress vpn import openvpn ipvanish ipvanish-fr /chemin/ipvanish` |
| Windscribe | OpenVPN `config.ovpn` y Get Credentials `auth.txt`, o WireGuard `config.conf`. | El inicio de sesión del sitio. IKEv2. | `sudo ./liltress vpn import openvpn windscribe windscribe-fr /chemin/windscribe-openvpn` |
| Perso | Archivo WireGuard de la página wg-easy del servidor, copiado como `config.conf`. | OpenVPN. Un archivo comprimido. | `sudo ./liltress vpn import wireguard perso poste-1 /chemin/perso.conf` |

Sustituya `REPLACE` en cada archivo del directorio, o en el archivo WireGuard, antes de la importación.<br/>
Surfshark, Proton VPN, PureVPN y Windscribe aceptan ambos protocolos, pero no ambos archivos en una misma importación.<br/>
El extremo de ejemplo de Perso es `203.0.113.10:51820`. La dirección de ejemplo es `10.8.0.2/24`.
