# Available functions

Extract from `system/sudoers/commands.csv`.

## `./liltress maintain scan-skip add <path>`

**Effect** : Adds a path to the ClamAV on-access exclusion list.<br/>
**File modified** : `config/service/clamav/scan-skip.paths`<br/>
**Success message** : `Chemin ajouté : <path>`<br/>
**Restrictions** : Refuses `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, the full home, Downloads, Desktop and their parent.

## `./liltress maintain scan-skip del <path>`

**Effect** : Removes a path from the exclusion list.<br/>
**Success message** : `Chemin retiré : <path>`

## `./liltress maintain aide`

**Effect** : Rebuilds the AIDE baseline database.<br/>
**Success message** : `Base de référence AIDE enregistrée.`

## `./liltress maintain rkhunter`

**Effect** : Recalculates the rkhunter property database.<br/>
**Success message** : `Base de propriétés rkhunter enregistrée.`

## `./liltress maintain chkrootkit <log>`

**Effect** : Records lines from a chkrootkit journal as ignored.<br/>
**Success message** : `Aucune nouvelle détection à enregistrer.` or `N détection(s) enregistrée(s) dans ...`

## `./liltress verify alerts`

**Effect** : Checks the alert chain and sends a probe when mail is enabled.<br/>
**Success message** : `Résultat : OK`

## `./liltress verify boot`

**Effect** : Checks boot security services.<br/>
**Success message** : `Contrôle de démarrage : OK`
