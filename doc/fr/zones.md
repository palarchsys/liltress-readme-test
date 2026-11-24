# Carte des zones surveillées

[English](../en/zones.md) · **Français** · [Deutsch](../de/zones.md) · [Español](../es/zones.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- [E-mails d'alerte](alerts.md)
- [Commandes après installation](commands.md)
- **Carte des zones surveillées**
- [VPN](vpn.md)

Les couleurs aident la lecture. Elles ne sont pas une seconde politique. Les outils de la ligne sont la politique.

| Zone | Niveau | Outils | Notes |
| --- | --- | --- | --- |
| `/home` hors exclusions livrées | 🔴 Haute | ClamAV à l'accès, ClamAV périodique, AIDE | Fichiers utilisateur. |
| `~/.cache`, `~/snap` | ⚪ Exclu | Exclusion à l'accès | Livré dans `config/service/clamav/scan-skip.paths`. Le scan périodique parcourt encore les chemins exclus de l'accès. |
| Téléchargements, Bureau | 🔴 Haute | ClamAV à l'accès | `scan-skip add` refuse ces chemins, leur parent, `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root` et le home entier. |
| `/etc` | 🟡 Standard | AIDE, auditd | Fichiers de configuration suivis par la base d'intégrité et les règles d'audit. |
| `/opt/liltress-workstation` | 🟠 Moyen | AIDE, journaux locaux | Arbre du produit, copies d'alertes et journaux de sécurité. |
| `/var/log` | 🟢 Léger | rotation des journaux, auditd | Les motifs `chkrootkit` ignorent `/var/log`. |
| Stockage USB | 🟡 Standard | usbguard | Un stockage nouveau attend une confirmation du bureau. Claviers, souris, hubs et périphériques déjà présents sont autorisés. |
| Fichiers empaquetés sous `/usr` | 🟢 Léger | debsums, rkhunter | Sommes de contrôle et base de propriétés. Un écart debsums peut réinstaller le paquet. |

## Légende

- 🔴 Haute : scan à l'accès plus intégrité, ou chemin qui ne peut pas être exclu.
- 🟠 Moyen : intégrité et arbre du produit.
- 🟡 Standard : configuration, audit, ou confirmation d'un périphérique.
- 🟢 Léger : sommes, propriétés ou journaux.
- ⚪ Exclu : nommé dans la liste d'exclusion à l'accès. Ce n'est pas un trou silencieux du scan périodique.

Changer un niveau signifie changer la configuration de l'outil (`scan-skip`, AIDE, watches auditd, usbguard), pas la couleur de ce tableau.
