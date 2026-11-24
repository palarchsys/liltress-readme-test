<div align="center">

# 🛡️ Fonctions disponibles 🛡️

</div>

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

---

## 📋 Le lanceur

Ces commandes préparent, installent ou inspectent le poste.

## ▪️ help

Affiche les commandes liltress.

```bash
./liltress help
```

Commandes reconnues : configure, check, run, lynis, logs, vpn, verify, maintain, timer, install, help.<br/>
-h et --help affichent la même aide.<br/>
Une commande inconnue affiche commande inconnue et sort avec le code 2.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/share/core/run/launch.sh`

## ▪️ configure

Prépare la configuration, puis installe.

```bash
sudo ./liltress configure
```

Le script pose les questions, écrit global.conf en mode 644 et secrets.conf en mode 600, puis lance script/run.sh.<br/>
Le jeton Ubuntu Pro est obligatoire.<br/>
Les alertes se répondent par Y ou n.

Si elles sont activées, le serveur SMTP, le login, le mot de passe d'application et l'adresse From sont demandés, et swaks doit recevoir un 250.<br/>
MODE_TEST se répond par 0 ou 1.<br/>
Le défaut livré est 1.

Une option inconnue est refusée.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Essai d'envoi accepté.`

Succès : `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

Source : `src/workstation/script/configure.sh`

## ▪️ check

Vérifie global.conf et secrets.conf sans les réécrire.

```bash
sudo ./liltress check
```

Le code 0 signifie que les deux fichiers sont conformes.<br/>
secrets.conf est en mode 600, donc sudo est requis pour le lire.<br/>
Équivaut à script/configure.sh --check.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Configuration conforme`

Source : `src/workstation/script/configure.sh`

## ▪️ run

Lance l'installation à partir des fichiers déjà préparés.

```bash
sudo ./liltress run
```

L'installeur refuse de démarrer si le contrôle de configuration échoue.<br/>
Il enchaîne la purge, les paquets, le durcissement, les contrôles de sécurité, les bases AIDE et rkhunter, puis verify/workstation.sh.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/script/run.sh`

## ▪️ lynis

Lance un audit Lynis du poste.

```bash
sudo ./liltress lynis
```

La commande réelle est lynis audit system --quick --no-colors.<br/>
Le rapport est écrit sous <source>/log/lynis-<date>/.<br/>
La dernière ligne affiche Rapport : suivi de ce dossier.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Rapport :`

Source : `src/share/core/run/lynis.sh`

## ▪️ logs

Copie un instantané des journaux dans les sources.

```bash
sudo ./liltress logs
```

Le dossier créé est <source>/log/snapshot-<date>-<hôte>.<br/>
Les fichiers privés (secrets.conf, clés, mots de passe) ne sont pas copiés.<br/>
Un fichier de plus de 8 Mio est tronqué, et la coupe est notée dans snapshot/truncated.txt.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/share/script/log-collect.sh`

## 📋 VPN

Ces commandes gèrent les profils.

## ▪️ vpn status

Affiche le mode VPN, le killswitch et le profil actif.

```bash
sudo ./liltress vpn status
```

Le mode est on ou off.<br/>
Le killswitch est armé ou désarmé.<br/>
Sans profil actif, la ligne le dit.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn list

Liste les profils VPN enregistrés.

```bash
sudo ./liltress vpn list
```

Chaque ligne donne le nom, le protocole, la validité, la date de validation, la date observée et la date du dernier essai.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import wireguard <marque> <nom> <dossier ou fichier>

Importe un profil WireGuard.

```bash
sudo ./liltress vpn import wireguard <marque> <nom> <dossier ou fichier>
```

Le fichier est copié sous /opt/liltress-workstation/vpn/wireguard/<nom>/.<br/>
Un essai valide le profil.<br/>
Le mode s'arme au premier profil valide.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import openvpn <marque> <nom> <dossier ou fichier>

Importe un profil OpenVPN.

```bash
sudo ./liltress vpn import openvpn <marque> <nom> <dossier ou fichier>
```

Les fichiers cités par le profil sont copiés.<br/>
Un auth-user-pass sans chemin reçoit auth.txt.<br/>
Le tunnel n'est pas ouvert si le fichier d'identifiants a moins de deux lignes.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn test <nom>

Reteste un profil et le revalide s'il répond.

```bash
sudo ./liltress vpn test <nom>
```

Trois dates sont tenues : validation, dernier état observé, dernier essai.<br/>
Un échec ne retire pas la date de validation.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn switch <nom>

Bascule vers un autre profil déjà validé.

```bash
sudo ./liltress vpn switch <nom>
```

La commande ne descend pas un profil.<br/>
Elle ne choisit qu'un profil encore valide.<br/>
Les services qui en dépendent, dont Unbound, sont relancés.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode on

Arme le mode VPN.

```bash
sudo ./liltress vpn mode on
```

Le tunnel et le killswitch restent jusqu'à un arrêt manuel.<br/>
SSH ne passe pas par le tunnel.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode off

Arrête le mode VPN.

```bash
sudo ./liltress vpn mode off
```

Le réseau redevient direct et le killswitch est retiré.<br/>
Un essai réussi ensuite ne réarme pas le mode.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn rm <nom>

Retire un profil VPN.

```bash
sudo ./liltress vpn rm <nom>
```

Le profil actif ne peut pas être retiré.<br/>
La commande efface le dossier et la ligne du registre.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass

Affiche les préfixes qui restent hors du tunnel.

```bash
sudo ./liltress vpn bypass
```

La liste lue est /opt/liltress-workstation/vpn/bypass.cidrs.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass reload

Recharge les préfixes hors tunnel.

```bash
sudo ./liltress vpn bypass reload
```

Les routes de contournement sont réappliquées si le killswitch est armé.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn refresh-endpoint <nom>

Recalcule l'adresse du serveur VPN.

```bash
sudo ./liltress vpn refresh-endpoint <nom>
```

Le nom DNS du profil est résolu à nouveau.<br/>
L'adresse obtenue remplace l'extrémité enregistrée.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/service/vpn/cli.sh`

Les marques, le killswitch et le profil de démarrage sont dans [vpn.md](vpn.md).

## 📋 Contrôles

Ces commandes contrôlent. Elles ne reconstruisent pas une base.

## ▪️ verify alerts

Contrôle la chaîne d'alertes et envoie une sonde.

```bash
sudo ./liltress verify alerts
```

La passe contrôle le timer de démarrage, le timer périodique, ClamAV à l'accès et le chemin du courrier.<br/>
Un seul message d'essai part lorsque les alertes sont activées.<br/>
Le sujet se termine par Contrôle.

-h et --help affichent l'usage.<br/>
Une autre option est refusée.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Résultat : OK`

Source : `src/workstation/script/verify/alerts.sh`

## ▪️ verify alerts --no-send

Même contrôle d'alertes, sans sonde.

```bash
sudo ./liltress verify alerts --no-send
```

Même périmètre que verify/alerts.sh.<br/>
Aucun message d'essai n'est envoyé.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Résultat : OK`

Source : `src/workstation/script/verify/alerts.sh`

## ▪️ verify workstation

Vérifie le poste après installation.

```bash
sudo ./liltress verify workstation
```

Le premier argument est le répertoire des sources.<br/>
Les lignes de clôture sont Check Success : N, Check Warnings : N et Check Fails : N.<br/>
La passe contrôle le relais SSH, la règle de pare-feu SSH, USBGuard, le délai sudo et w-reboot-notice.timer.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Check Success : N`

Source : `src/workstation/script/verify/workstation.sh`

## ▪️ verify boot-check

Vérifie les services de sécurité de ce démarrage.

```bash
sudo ./liltress verify boot-check
```

Une passe encore en cours affiche Contrôle de démarrage : en cours et sort en 1.<br/>
Un écart affiche Contrôle de démarrage : écarts et sort en 1.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Contrôle de démarrage : OK`

Source : `src/workstation/script/boot-check.sh`

## ▪️ verify boot

Lance boot-check.sh depuis les sources.

```bash
sudo ./liltress verify boot
```

script/verify/boot.sh exécute script/boot-check.sh du même arbre.<br/>
Le résultat est celui de boot-check.sh.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Succès : `Contrôle de démarrage : OK`

Source : `src/workstation/script/verify/boot.sh`

## ▪️ verify chkrootkit

Relève une passe chkrootkit pour lecture humaine.

```bash
sudo ./liltress verify chkrootkit
```

Le journal est /var/log/liltress-workstation/chkrootkit-fp-<date>.log.<br/>
Cette commande n'enregistre rien dans la liste d'ignorés.

Lancez-la depuis `/opt/liltress-workstation`, ou depuis le clone.

Source : `src/workstation/script/verify/chkrootkit-fp.sh`

## 📋 Maintenance

Ces commandes reconstruisent une base, enregistrent une liste d'ignorés, ou remettent un fichier.

## ▪️ maintain aide

Reconstruit la base AIDE.

```bash
sudo ./liltress maintain aide
```

La commande lance aide --init et remplace /var/lib/aide/aide.db.<br/>
Le calcul peut prendre plusieurs minutes.<br/>
C'est la commande jointe au courrier AIDE.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Base de référence AIDE enregistrée.`

Source : `src/workstation/service/aide/refresh-db.sh`

Installé : `/opt/liltress-workstation/service/aide/refresh-db.sh`

## ▪️ maintain rkhunter

Recalcule la base de propriétés rkhunter.

```bash
sudo ./liltress maintain rkhunter
```

La commande lance rkhunter --propupd --nocolors.<br/>
C'est la commande jointe au courrier d'avertissements rkhunter.<br/>
Le courrier mirrors.dat modifié hors rkhunter --update ne la joint pas.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Base de propriétés rkhunter enregistrée.`

Source : `src/share/service/rkhunter/refresh-db.sh`

Installé : `/opt/liltress-workstation/service/rkhunter/refresh-db.sh`

## ▪️ maintain chkrootkit <journal>

Enregistre les lignes d'un journal chkrootkit comme ignorées.

```bash
sudo ./liltress maintain chkrootkit <journal>
```

Le fichier doit être un chkrootkit-DATE.log déjà écrit sous cron/security_logs/.<br/>
Les lignes nouvelles sont ajoutées à /opt/liltress-workstation/cron/chkrootkit.local.ignore.<br/>
S'il n'y a rien de nouveau, la ligne est Aucune nouvelle détection à enregistrer.

Sinon N détection(s) enregistrée(s) dans ce fichier.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Aucune nouvelle détection à enregistrer.`

Succès : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`

Source : `src/workstation/service/chkrootkit/ignore-log.sh`

Installé : `/opt/liltress-workstation/service/chkrootkit/ignore-log.sh`

## ▪️ maintain debsums <journal>

Enregistre les lignes d'un journal debsums comme ignorées.

```bash
sudo ./liltress maintain debsums <journal>
```

Le fichier doit être un debsums-DATE.log sous cron/security_logs/.<br/>
Les motifs vont dans /opt/liltress-workstation/cron/debsums.local.ignore.<br/>
Les messages de succès sont les mêmes que pour chkrootkit.

Le courrier debsums ne propose pas cette commande : le contrôle réinstalle déjà chaque paquet encore signalé.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Aucune nouvelle détection à enregistrer.`

Succès : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/debsums.local.ignore`

Source : `src/workstation/service/debsums/ignore-log.sh`

Installé : `/opt/liltress-workstation/service/debsums/ignore-log.sh`

## ▪️ maintain scan-skip list

Liste les chemins exclus de l'accès ClamAV.

```bash
sudo ./liltress maintain scan-skip list
```

Un chemin accepté est affiché par ligne.<br/>
Un chemin refusé est écrit sur l'erreur : Chemin refusé : <path>.<br/>
Le fichier est /opt/liltress-workstation/config/service/clamav/scan-skip.paths.

Elle peut partir de n'importe quel répertoire de la machine.

Source : `src/workstation/script/scan-skip.sh`

Installé : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip add <chemin>

Ajoute un chemin exclu de l'accès ClamAV.

```bash
sudo ./liltress maintain scan-skip add <chemin>
```

S'il est déjà présent : Ce chemin est déjà exclu : <path>.<br/>
Un refus laisse le fichier inchangé et affiche Chemin refusé : <path>.<br/>
Un chemin est accepté sous le home du compte du poste, plus bas que ce home.

/, /home, /opt, /tmp, /etc, /usr, /var, /boot, /root, le home entier, Téléchargements, Bureau et leur parent sont refusés.<br/>
Après un changement, la commande réécrit le bloc d'exclusion de /etc/clamav/clamd.conf et relance clamav-daemon, puis clamav-clamonacc.service ou clamonacc.service.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Chemin ajouté : <path>`

Source : `src/workstation/script/scan-skip.sh`

Installé : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip del <chemin>

Retire un chemin de la liste d'exclusion ClamAV.

```bash
sudo ./liltress maintain scan-skip del <chemin>
```

S'il est absent, la commande affiche Ce chemin n'est pas dans la liste : <path> et sort en 1.<br/>
La même réécriture de clamd.conf suit un retrait réussi.

Elle peut partir de n'importe quel répertoire de la machine.

Succès : `Chemin retiré : <path>`

Source : `src/workstation/script/scan-skip.sh`

Installé : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain mirrors save

Enregistre l'empreinte des miroirs rkhunter.

```bash
sudo ./liltress maintain mirrors save
```

Le SHA-256 de /var/lib/rkhunter/db/mirrors.dat est écrit dans /opt/liltress-workstation/cron/rkhunter-mirrors.sha256, en mode 600.<br/>
refresh-rkhunter-db.sh fait cet enregistrement seulement après un rkhunter --update qui retourne 0 ou 2.

Elle peut partir de n'importe quel répertoire de la machine.

Source : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Installé : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain mirrors check

Compare l'empreinte rkhunter actuelle à celle enregistrée.

```bash
sudo ./liltress maintain mirrors check
```

Aucun texte n'est affiché.<br/>
Le code 0 signifie que les deux empreintes sont égales, ou qu'aucune empreinte n'est encore enregistrée.<br/>
Un écart sort avec un code différent de 0.

Elle peut partir de n'importe quel répertoire de la machine.

Source : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Installé : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain restore <id>

Remet un fichier de quarantaine à son emplacement d'origine.

```bash
sudo ./liltress maintain restore <id>
```

L'identifiant est une ligne du registre cron/quarantine.registry.<br/>
Le script est core/quarantine/restore.sh.

Elle peut partir de n'importe quel répertoire de la machine.

Source : `src/share/core/quarantine/restore.sh`

Installé : `/opt/liltress-workstation/core/quarantine/restore.sh`

## ▪️ maintain mail

Envoie un courrier d'essai.

```bash
sudo ./liltress maintain mail
```

Le message sert à voir la largeur et la hauteur.<br/>
Le destinataire reste celui de la configuration installée.<br/>
Le script est core/mail/mail-test.sh.

Elle peut partir de n'importe quel répertoire de la machine.

Source : `src/share/core/mail/mail-test.sh`

Installé : `/opt/liltress-workstation/core/mail/mail-test.sh`
