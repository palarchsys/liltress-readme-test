# Fonctions disponibles

[English](../en/functions.md) · **Français** · [Deutsch](../de/functions.md) · [Español](../es/functions.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- **Fonctions disponibles**
- [E-mails d'alerte](alerts.md)
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)

Chaque commande ci-dessous est une entrée réelle de `./liltress`. Lancez-les depuis `/opt/liltress-workstation`, sauf mention contraire.

Le texte de succès est la ligne imprimée par le script. Il n'est pas traduit.

## liltress

| Commande | Effet | Succès ou sortie |
| --- | --- | --- |
| `./liltress help` | Liste les commandes reconnues : `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`. `-h` et `--help` affichent la même aide. | Une commande inconnue affiche la ligne de commande inconnue et sort en `2`. |
| `sudo ./liltress configure` | Pose les questions, écrit `global.conf` en mode `644` et `secrets.conf` en mode `600`, puis lance `script/run.sh`. | Essai mail : `Essai d'envoi accepté.` Courrier coupé : `Alertes e-mail coupées. Les contrôles partiront sans envoi.` |
| `sudo ./liltress check` | Contrôle `global.conf` et `secrets.conf` sans les réécrire. Équivaut à `script/configure.sh --check`. | `Configuration conforme` |
| `sudo ./liltress run` | Lance l'installation depuis les fichiers préparés. Refuse de démarrer si le contrôle échoue. Enchaîne la purge, les paquets, le durcissement, les contrôles, les bases AIDE et rkhunter, puis `verify workstation`. | L'installeur s'arrête si le contrôle échoue. |
| `sudo ./liltress lynis` | Lance `lynis audit system --quick --no-colors`. Écrit le rapport sous `<source>/log/lynis-<date>/`. | La dernière ligne commence par `Rapport :` |
| `sudo ./liltress logs` | Copie un instantané dans `<source>/log/snapshot-<date>-<hôte>`. Les fichiers privés (`secrets.conf`, clés, mots de passe) ne sont pas copiés. Un fichier de plus de 8 Mio est tronqué, et la coupe est notée dans `snapshot/truncated.txt`. | Le dossier d'instantané est le résultat. |

## vpn

| Commande | Effet |
| --- | --- |
| `sudo ./liltress vpn status` | Affiche le mode `on` ou `off`, le killswitch armé ou non, et le profil actif. |
| `sudo ./liltress vpn list` | Liste les profils : nom, protocole, validité, date de validation, date observée, dernier essai. |
| `sudo ./liltress vpn import wireguard <brand> <name> <directory or file>` | Copie le profil sous `/opt/liltress-workstation/vpn/wireguard/<name>/` et le teste. Le premier profil valide arme le mode. |
| `sudo ./liltress vpn import openvpn <brand> <name> <directory or file>` | Copie les fichiers cités. Un `auth-user-pass` sans chemin reçoit `auth.txt`. Le tunnel ne s'ouvre pas si le fichier d'identifiants a moins de deux lignes. |
| `sudo ./liltress vpn test <name>` | Reteste un profil. Un échec ne retire pas la date de validation. |
| `sudo ./liltress vpn switch <name>` | Bascule vers un autre profil encore valide. Ne descend pas un profil. Relance les services qui en dépendent, dont Unbound. |
| `sudo ./liltress vpn mode on` | Arme le mode VPN. Le tunnel et le killswitch restent jusqu'à un arrêt manuel. SSH ne passe pas par le tunnel. |
| `sudo ./liltress vpn mode off` | Arrête le mode VPN. Le réseau redevient direct et le killswitch est retiré. Un essai réussi ensuite ne réarme pas le mode. |
| `sudo ./liltress vpn rm <name>` | Retire un profil. Le profil actif ne peut pas être retiré. Efface le dossier et la ligne du registre. |
| `sudo ./liltress vpn bypass` | Affiche les préfixes hors tunnel. Le fichier est `/opt/liltress-workstation/vpn/bypass.cidrs`. |
| `sudo ./liltress vpn bypass reload` | Recharge ces préfixes. Les routes de contournement sont réappliquées si le killswitch est armé. |
| `sudo ./liltress vpn refresh-endpoint <name>` | Résout à nouveau le nom DNS et remplace l'extrémité enregistrée. |
| `sudo ./liltress vpn default` | Affiche le profil de démarrage, ou aucun. |
| `sudo ./liltress vpn default <name>` | Choisit un profil valide comme VPN de démarrage. La session en cours ne change pas. |
| `sudo ./liltress vpn default off` | Efface ce choix. Un tunnel ouvert reste monté. |

Le détail des imports, des marques et du killswitch est dans [vpn.md](vpn.md).

## verify

| Commande | Effet | Succès ou sortie |
| --- | --- | --- |
| `sudo ./liltress verify alerts` | Contrôle le timer de démarrage, le timer périodique, ClamAV à l'accès et le chemin du courrier. Envoie une sonde si les alertes sont activées. Le sujet se termine par `Contrôle`. | `Résultat : OK` |
| `sudo ./liltress verify alerts --no-send` | Même contrôle. Aucune sonde. | `Résultat : OK` |
| `sudo ./liltress verify workstation` | Contrôle après installation. Contrôle aussi le relais SSH, la règle de pare-feu SSH, USBGuard, le délai sudo et `w-reboot-notice.timer`. | `Check Success : N`, `Check Warnings : N`, `Check Fails : N` |
| `sudo ./liltress verify boot` | Lance `script/boot-check.sh` depuis les sources. | `Contrôle de démarrage : OK` |
| `sudo ./liltress verify boot-check` | Même contrôle depuis n'importe quel répertoire de la machine. | En cours : `Contrôle de démarrage : en cours`, sortie `1`. Écart : `Contrôle de démarrage : écarts`, sortie `1`. |
| `sudo ./liltress verify chkrootkit` | Relève une passe chkrootkit pour lecture humaine. Journal : `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`. | N'écrit pas la liste d'ignorés. |

## maintain

| Commande | Effet | Ligne de succès |
| --- | --- | --- |
| `sudo ./liltress maintain aide` | Lance `aide --init` et remplace `/var/lib/aide/aide.db`. Peut prendre plusieurs minutes. | `Base de référence AIDE enregistrée.` |
| `sudo ./liltress maintain rkhunter` | Lance `rkhunter --propupd --nocolors`. | `Base de propriétés rkhunter enregistrée.` |
| `sudo ./liltress maintain chkrootkit <journal>` | Enregistre les lignes nouvelles d'un `chkrootkit-DATE.log` déjà sous `cron/security_logs/`. | `Aucune nouvelle détection à enregistrer.` ou `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore` |
| `sudo ./liltress maintain debsums <journal>` | Enregistre les lignes d'un `debsums-DATE.log` dans `/opt/liltress-workstation/cron/debsums.local.ignore`. Le courrier debsums ne propose pas cette commande : le contrôle réinstalle déjà chaque paquet encore signalé. | Les deux mêmes formes, avec `debsums.local.ignore`. |
| `sudo ./liltress maintain scan-skip list` | Liste les chemins exclus de l'accès ClamAV. Fichier : `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`. | Un chemin accepté par ligne. Chemin refusé sur l'erreur : `Chemin refusé : <path>`. |
| `sudo ./liltress maintain scan-skip add <path>` | Ajoute une exclusion, réécrit le bloc de `/etc/clamav/clamd.conf`, relance `clamav-daemon`, puis `clamav-clamonacc.service` ou `clamonacc.service`. | `Chemin ajouté : <path>` |
| `sudo ./liltress maintain scan-skip del <path>` | Retire une exclusion et réécrit `clamd.conf` de la même façon. | `Chemin retiré : <path>` |
| `sudo ./liltress maintain mirrors save` | Écrit le SHA-256 de `/var/lib/rkhunter/db/mirrors.dat` dans `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, mode `600`. | Pas de bandeau fixe. Le fichier est le résultat. |
| `sudo ./liltress maintain mirrors check` | Compare l'empreinte actuelle à celle enregistrée. N'affiche rien. | Sortie `0` si elles sont égales, ou si aucune empreinte n'est encore enregistrée. Toute autre sortie signifie un écart. |
| `sudo ./liltress maintain restore <id>` | Remet un fichier de quarantaine. L'identifiant est une ligne de `cron/quarantine.registry`. Script : `core/quarantine/restore.sh`. | `fichier remis en place` |
| `sudo ./liltress maintain mail` | Envoie un essai de mise en page au destinataire configuré. Script : `core/mail/mail-test.sh`. | Le message d'essai est le résultat. |

### Limites de scan-skip

Un chemin est accepté sous le home du compte du poste, et plus bas que ce home.

`/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, le home entier, Téléchargements, Bureau et leur parent sont refusés.

Déjà présent : `Ce chemin est déjà exclu : <path>`.

Absent au retrait : `Ce chemin n'est pas dans la liste : <path>`, sortie `1`.

Un chemin refusé laisse le fichier inchangé.
