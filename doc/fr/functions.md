# Fonctions disponibles

Extrait de `system/sudoers/commands.csv`.

## `./liltress maintain scan-skip add <chemin>`

**Effet** : Ajoute un chemin à la liste d’exclusion ClamAV à l’accès.<br/>
**Fichier modifié** : `config/service/clamav/scan-skip.paths`<br/>
**Message de succès** : `Chemin ajouté : <path>`<br/>
**Restrictions** : Refuse `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, le home entier, Téléchargements, Bureau et leur parent.

## `./liltress maintain scan-skip del <chemin>`

**Effet** : Retire un chemin de la liste d’exclusion.<br/>
**Message de succès** : `Chemin retiré : <path>`

## `./liltress maintain aide`

**Effet** : Reconstruit la base de référence AIDE.<br/>
**Message de succès** : `Base de référence AIDE enregistrée.`

## `./liltress maintain rkhunter`

**Effet** : Recalcule la base de propriétés rkhunter.<br/>
**Message de succès** : `Base de propriétés rkhunter enregistrée.`

## `./liltress maintain chkrootkit <journal>`

**Effet** : Enregistre les lignes d’un journal chkrootkit comme ignorées.<br/>
**Message de succès** : `Aucune nouvelle détection à enregistrer.` ou `N détection(s) enregistrée(s) dans ...`

## `./liltress verify alerts`

**Effet** : Contrôle la chaîne d’alertes et envoie une sonde lorsque le courrier est activé.<br/>
**Message de succès** : `Résultat : OK`

## `./liltress verify boot`

**Effet** : Vérifie les services de sécurité de ce démarrage.<br/>
**Message de succès** : `Contrôle de démarrage : OK`
