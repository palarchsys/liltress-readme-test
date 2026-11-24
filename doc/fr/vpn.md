# VPN

[English](../en/vpn.md) · **Français** · [Deutsch](../de/vpn.md) · [Español](../es/vpn.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- [E-mails d'alerte](alerts.md)
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- **VPN**

L'installation du poste démarre sans VPN. Le réseau reste direct.

## Commandes

```bash
sudo ./liltress vpn import openvpn <brand> <name> <directory or file>
sudo ./liltress vpn import wireguard <brand> <name> <directory or file>
```

La marque est obligatoire. Un fichier sans marque est refusé.

Le premier profil valide devient le VPN de démarrage et arme le killswitch.

Un lot invalide n'est pas ajouté. S'il est le premier, le killswitch reste coupé.

Un import ultérieur, mode coupé, ne remplace pas le VPN de démarrage et ne réarme pas le killswitch.

Au démarrage, un VPN de démarrage valide arme le killswitch, puis ouvre ce profil.

```bash
sudo ./liltress vpn default
sudo ./liltress vpn default <name>
sudo ./liltress vpn default off
```

`vpn default` affiche ce profil, ou aucun.

`vpn default <name>` le sélectionne s'il est valide. La session en cours ne change pas.

`vpn default off` efface ce choix. Un tunnel ouvert reste monté.

```bash
sudo ./liltress vpn mode on
sudo ./liltress vpn mode off
```

`vpn mode on` arme le killswitch et ouvre un profil valide. Sans profil valide, la commande est refusée et le killswitch reste coupé.

`vpn mode off` désarme le killswitch et revient au réseau direct. Le VPN de démarrage reste pour le prochain démarrage. Les dossiers de profils restent.

Le profil apparaît dans Paramètres, Réseau. L'interrupteur est le mode. L'import n'est pas le bouton `+`.

```bash
sudo ./liltress vpn switch <name>
```

Bascule vers un autre profil valide. Le killswitch reste armé pendant la bascule. Le VPN de démarrage ne change pas.

Une bascule en échec ne rend pas Internet direct et ne retire pas un profil.

Un profil qui a déjà répondu, puis échoue trois essais de dix secondes, est retiré.

Lorsque ce retrait enlève le dernier profil, le réseau redevient direct et le killswitch est désarmé. Le courrier d'alerte part avant ce désarmement.

```bash
sudo ./liltress vpn rm <name>
sudo ./liltress vpn test <name>
sudo ./liltress vpn list
sudo ./liltress vpn status
```

`vpn rm` retire un profil. Retirer le VPN de démarrage essaie le profil suivant de la liste, après un contrôle. Retirer le dernier efface le VPN de démarrage et désarme le killswitch.

Un profil actif qui n'est pas le VPN de démarrage reste tant qu'un autre profil existe.

Un `vpn test` en échec laisse le dossier.

Chaque profil WireGuard est `/opt/liltress-workstation/vpn/wireguard/<name>/`.

Chaque profil OpenVPN est `/opt/liltress-workstation/vpn/openvpn/<name>/`.

Le fichier `meta` de ce dossier enregistre la marque.

Le registre est `/opt/liltress-workstation/vpn/vpn.json`.

`vpn list` affiche le nom, le protocole, la marque et les dates.

SSH reste hors du tunnel.

Le fichier de contournement contient un préfixe par ligne, ou une ligne entre crochets. L'exemple est `[192.168.1.0/24, 200.200.200.0/24]`.

```bash
sudo ./liltress vpn bypass
sudo ./liltress vpn bypass reload
sudo ./liltress vpn refresh-endpoint <name>
```

Unbound est relancé après un changement de mode ou une bascule.

L'IPv6 global est bloqué lorsque le tunnel n'a pas d'IPv6.

Une plage publique écrite dans le fichier de contournement sort en clair.

Après le retrait du dernier profil, le profil valide suivant redevient le VPN de démarrage et arme le killswitch.

## Formes d'import

Le dossier ou le fichier porte exactement les noms de fichiers de la marque.

Un fichier en trop, un fichier manquant, ou le fichier de l'autre protocole est refusé.

Un seul dossier est ouvert. `auth.txt` peut se trouver à côté de ce dossier.

Le journal du jour est `log/vpn/YYYY-MM-DD.log`.

Si le mot `REPLACE` est encore présent, l'import s'arrête avant la poignée de main.

L'adresse d'exemple `203.0.113.10` ne peut pas ouvrir un tunnel.

`auth.txt` a deux lignes : le nom, puis le secret.

Un certificat écrit dans le fichier reste dans le profil.

Un certificat nommé à côté du fichier est copié sous ce nom.

Les scripts `up` et `down` du fournisseur ne sont pas copiés.

Le DNS du fournisseur n'est pas utilisé. Unbound reste le résolveur.

Le premier `remote` est le serveur qui est ouvert.

## Marques

| Marque | Accepté | Refusé | Commande d'exemple |
| --- | --- | --- | --- |
| NordVPN | OpenVPN, certificats dans le `.ovpn`. `auth.txt` : nom de service puis mot de passe de service. | E-mail du compte. WireGuard. NordLynx. | `sudo ./liltress vpn import openvpn nordvpn nord-fr /chemin/nordvpn` |
| ExpressVPN | Un `.ovpn` de la page de configuration manuelle. `auth.txt` : identifiant et mot de passe affichés sur cette page. | WireGuard. Lightway. | `sudo ./liltress vpn import openvpn expressvpn express-fr /chemin/expressvpn` |
| Surfshark | OpenVPN avec `config.ovpn` et `auth.txt`, ou WireGuard `config.conf` seul. | Le fichier de l'autre protocole. L'e-mail du site. | `sudo ./liltress vpn import openvpn surfshark surf-fr /chemin/surfshark-openvpn` |
| Proton VPN | `config.ovpn` Linux plus identifiant et secret OpenVPN, ou WireGuard `config.conf` seul. | Le mot de passe de la boîte. Le fichier de l'autre protocole. | `sudo ./liltress vpn import wireguard protonvpn proton-fr /chemin/protonvpn-wireguard.conf` |
| CyberGhost | `openvpn.ovpn`, `ca.crt`, `client.crt`, `client.key`, `auth.txt`. | WireGuard. Un certificat manquant. L'identifiant du compte. | `sudo ./liltress vpn import openvpn cyberghost cyberghost-fr /chemin/cyberghost` |
| Private Internet Access | `config.ovpn` et `auth.txt`. Le nom est le nom `p` suivi des chiffres. | WireGuard. Un nom SOCKS. | `sudo ./liltress vpn import openvpn pia pia-fr /chemin/pia` |
| Mullvad | Fichier WireGuard copié en `config.conf`. Pas de `auth.txt`. | OpenVPN. | `sudo ./liltress vpn import wireguard mullvad mullvad-se /chemin/mullvad.conf` |
| PureVPN | OpenVPN avec `ca`, `cert`, `key` lorsqu'ils sont cités, plus identifiant et secret VPN. Ou WireGuard `config.conf`. | L'e-mail. Le fichier de l'autre protocole. Un WireGuard non activé sous trente minutes échoue la poignée de main et n'est pas publié. | `sudo ./liltress vpn import openvpn purevpn pure-fr /chemin/purevpn-openvpn` |
| IPVanish | `config.ovpn` et `auth.txt`. Certificats dans le fichier, ou à côté lorsqu'ils sont nommés. | WireGuard. | `sudo ./liltress vpn import openvpn ipvanish ipvanish-fr /chemin/ipvanish` |
| Windscribe | OpenVPN `config.ovpn` et `auth.txt` Get Credentials, ou WireGuard `config.conf`. | L'identifiant du site. IKEv2. | `sudo ./liltress vpn import openvpn windscribe windscribe-fr /chemin/windscribe-openvpn` |
| Perso | Fichier WireGuard de la page wg-easy du serveur, copié en `config.conf`. | OpenVPN. Une archive. | `sudo ./liltress vpn import wireguard perso poste-1 /chemin/perso.conf` |

Remplacez `REPLACE` dans chaque fichier du dossier, ou dans le fichier WireGuard, avant l'import.

Surfshark, Proton VPN, PureVPN et Windscribe acceptent les deux protocoles, mais pas les deux fichiers dans un seul import.

L'extrémité de l'exemple Perso est `203.0.113.10:51820`. L'adresse d'exemple est `10.8.0.2/24`.
