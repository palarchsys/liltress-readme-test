<div align="center">

# 🛡️ Logiciels et services 🛡️

</div>

[English](../en/software.md) · **Français** · [Deutsch](../de/software.md) · [Español](../es/software.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- **Logiciels et services**
- [Fonctions disponibles](functions.md)
- [E-mails d'alerte](alerts.md)
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)

---

## 📋 Ce guide

L'installation place les paquets et les services ci-dessous. Chacun est configuré depuis `config/service/<name>/` et le script correspondant sous `service/`.

| Nom | Rôle | Ce que le poste change |
| --- | --- | --- |
| openssh-server | Serveur SSH | La connexion root est refusée. `PasswordAuthentication yes`. `AllowTcpForwarding no`. `AllowAgentForwarding no`. |
| ufw | Pare-feu hôte | La politique entrante est le refus. La règle SSH reste explicite. |
| unbound | Résolveur DNS local | Écoute sur `127.0.0.1`, port 53. Le DNS d'un profil VPN n'est pas utilisé. |
| usbguard, usbguard-notifier | Politique USB | Les périphériques déjà branchés, plus les claviers, souris et hubs, sont autorisés. Un stockage nouveau attend une confirmation du bureau. |
| fail2ban | Bannissements | Les échecs d'authentification répétés sont bannis. |
| auditd | Piste d'audit | Écrit les règles des chemins sélectionnés. |
| aide | Intégrité des fichiers | Base `/var/lib/aide/aide.db`. Reconstruction : `./liltress maintain aide`. |
| rkhunter | Rootkit et propriétés | Base de propriétés : `./liltress maintain rkhunter`. `/dev/shm` et la socket `/dev/log` sont autorisés. |
| chkrootkit | Contrôle de rootkit | Binaire `/usr/sbin/chkrootkit`. Les lignes bruyantes connues peuvent être enregistrées depuis un journal. |
| clamav | Scan de logiciels malveillants | Scan à l'accès, scan de démarrage, scan périodique chaque heure. Une détection peut déplacer le fichier en quarantaine. |
| crowdsec | Décisions locales | Décisions locales à partir des acquisitions livrées. |
| debsums | Sommes des paquets | Un paquet encore signalé est réinstallé avec `apt-get install --reinstall`. |
| wireguard-tools, openvpn, conntrack | Un tunnel | Reste coupé jusqu'à un profil valide. SSH n'entre pas dans le tunnel. |
| apparmor | Profils de la distribution | `service/apparmor/userns.sh` retire un profil hfu qui n'est pas livré. |
| unattended-upgrades | Mises à jour de sécurité | Origines de sécurité. `Automatic-Reboot` vaut `false`. |
| postfix | Courrier d'alerte | Installé seulement lorsque `MAIL_ALERTS=1`. |
| lynis | Audit de durcissement | `LYNIS_MIN_SCORE` vaut 80. `verify workstation` accepte un `hardening_index` supérieur ou égal à ce seuil. |
| ubuntu-pro-client | Attache Ubuntu Pro | Attache le jeton. Une version non LTS n'active pas esm-infra, esm-apps ni Livepatch. Sur une LTS, un échec de ces activations n'arrête pas l'installation. |
| acct, sysstat, libpam-pwquality | Comptabilité et mots de passe | Comptabilité des processus, statistiques, qualité des mots de passe. |
| rng-tools-debian, haveged | Entropie | Sources d'entropie supplémentaires. |
| apt-listchanges, needrestart, apt-show-versions | Avis de paquets | Avis de changements et contrôles de redémarrage. Le courrier de redémarrage ne redémarre pas la machine. |

Un fichier de service n'est pas une seconde valeur d'une variable de contrat. Les valeurs vivent dans `global.conf`, `user.conf` ou `secrets.conf`.
