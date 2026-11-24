<div align="center">

# 🛡️ E-mails d'alerte 🛡️

</div>

[English](../en/alerts.md) · **Français** · [Deutsch](../de/alerts.md) · [Español](../es/alerts.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- **E-mails d'alerte**
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)

---

Chaque fichier d'alerte est stocké sous `/opt/liltress-workstation/cron/alerts`.<br/>
Le courrier suit `LANGUAGE`.<br/>
Le courrier part seulement lorsque `MAIL_ALERTS=1`, `WATCHDOG_MAIL` est défini, et `/opt/liltress-workstation/service/cron/watchdogs/send.sh` est exécutable.

Un contrôle propre n'envoie pas de courrier.<br/>
Si une ligne de journal est inconnue, ne lancez pas la commande. Conservez le courrier.<br/>
Un contrôle qui s'arrête avant sa fin normale envoie un courrier. Le courrier nomme le contrôle, et le signal ou le code de sortie.

## 📋 AIDE

Le message contient le résumé, un extrait du journal, un bloc de vérification par constat, puis la note et la commande.<br/>
La forme du journal est `/opt/liltress-workstation/cron/security_logs/aide-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain aide
```

La commande reconstruit la base AIDE à partir du disque actuel.<br/>
La ligne de succès est `Base de référence AIDE enregistrée.`

## 📋 chkrootkit

Le message contient le résumé, l'extrait, un bloc de vérification par constat, puis la note et la commande.

```bash
sudo ./liltress maintain chkrootkit '/opt/liltress-workstation/cron/security_logs/chkrootkit-YYYYMMDD-HHMMSS.log'
```

La commande ajoute les lignes nouvelles à `/opt/liltress-workstation/cron/chkrootkit.local.ignore`.<br/>
Le succès est `Aucune nouvelle détection à enregistrer.` ou `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`.<br/>
Une détection confirmée déplace le fichier et joint la commande de restauration à la place. La commande d'enregistrement ne reste que si aucun fichier confirmé n'a été déplacé.

## 📋 debsums

Le message contient le résumé et l'extrait. Il n'ajoute pas de bloc de commande.<br/>
Le contrôle réinstalle chaque paquet encore signalé avec `apt-get install --reinstall`.<br/>
Le courrier nomme chaque paquet restauré.

La ligne de succès est `La somme correspond.`<br/>
Un paquet dont la somme reste différente, ou dont la réinstallation échoue, reste dans le courrier.<br/>
Une ligne pour `/var/lib/rkhunter/db/mirrors.dat` est retirée avant cette réinstallation seulement lorsque son SHA-256 actuel égale `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`.

Une ligne modifiée sous `/usr/lib/*/perl-base/unicore/` du paquet `perl-base` est retirée avant cette réinstallation. Le fichier livré par Ubuntu ne correspond pas à cette somme.

## 📋 ClamAV à l'accès

Le message contient la signature, le nom du fichier, le chemin d'origine, le chemin de quarantaine, l'heure, le type de détection, les actions, une courte recommandation, et une commande de restauration par fichier déplacé.<br/>
Un fichier dont le nom correspond à `.clamav-quarantine-lock.*` n'envoie pas de courrier. Le script sort en `0` avant le journal et avant `log_alert`.

```bash
sudo bash /opt/liltress-workstation/core/quarantine/restore.sh <id>
```

La ligne de succès est `fichier remis en place`.<br/>
Le registre est `/var/lib/liltress-workstation/watchdog/quarantine.registry`.

## 📋 Scan ClamAV de démarrage et scan périodique

Les deux messages contiennent le résumé et l'extrait. Un fichier déplacé ajoute une commande de restauration, la même que pour l'accès.<br/>
La forme du journal est `/opt/liltress-workstation/cron/security_logs/clamav-YYYYMMDD-HHMMSS.log`.<br/>
La chaîne périodique tourne toutes les 1 heure. Le même scan tourne aussi une fois dans la chaîne de démarrage.

Le parcours périodique inclut les chemins exclus du scan à l'accès.<br/>
Un fichier illisible, sans ligne `FOUND`, n'envoie pas de courrier. Les lignes `Known viruses` et `Infected files` ne sont pas copiées dans le courrier.<br/>
Un `clamscan` arrêté par un signal envoie un courrier. Le courrier nomme le signal et chaque fichier non lu. Les autres fichiers de la passe sont scannés. Deux plantages de suite arrêtent la passe ClamAV. Le courrier dit alors que les fichiers restants n'ont pas été lus.

## 📋 rkhunter

Le courrier d'avertissement contient le résumé, l'extrait, un bloc de vérification par constat, puis la note et la commande.<br/>
Le courrier `mirrors.dat modifié hors rkhunter --update` n'ajoute pas ce bloc. Un courrier d'erreur non plus.<br/>
La forme du journal est `/opt/liltress-workstation/cron/security_logs/rkhunter-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain rkhunter
```

La ligne de succès est `Base de propriétés rkhunter enregistrée.`<br/>
Une détection confirmée déplace le fichier et joint la commande de restauration à la place.<br/>
Les fichiers de `/dev/shm` et la socket `/dev/log` sont autorisés dans `rkhunter.conf.local`. Ils ne produisent pas `Suspicious file types found in /dev`.

## 📋 Avis de redémarrage

Le message est `Un redémarrage est utile. La machine n'a pas été redémarrée.`<br/>
Ce courrier n'ajoute pas de bloc de commande.<br/>
Le même ensemble de paquets n'envoie pas le courrier une seconde fois.

Le script ne redémarre pas la machine.<br/>
La forme du journal est `/opt/liltress-workstation/cron/security_logs/reboot-notice-YYYYMMDD-HHMMSS.log`.
