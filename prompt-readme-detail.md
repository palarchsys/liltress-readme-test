# Prompt — READMEs Liltress Workstation, version détaillée

Génère de nouveau le pack de documentation du dépôt de test public `palarchsys/liltress-readme-test`.
La source des faits est le dépôt `palarchsys/liltress-workstation`.
La loi de forme est `agents/laws/readme.md` du dépôt mère `palarchsys/liltress`, sauf là où ce prompt la contredit.
Ce prompt contredit la loi sur un seul point : le guide du poste de test est découpé en fichiers, avec le même menu sur chaque page. Ne reviens pas à un seul README dense.

Anglais = source. `fr`, `de`, `es` traduisent le même plan, dans le même ordre.
Ne traduis jamais une commande, un chemin, un nom de variable, un nom de fichier, ni une ligne de succès.

## Ce qui doit rester

Le menu actuel est bon. Ne le remplace pas.

Sur chaque page `doc/<lang>/*.md`, dans cet ordre :

1. Bloc centré : `# 🛡️ <titre de la page> 🛡️`.
2. Ligne de langue. La langue courante est en gras, sans lien. Les trois autres sont des liens relatifs `../<code>/<même fichier>`. Ordre : English, Français, Deutsch, Español.
3. `## Navigation`, puis exactement ces neuf entrées. L'entrée de la page courante est en gras, sans lien.

| Fichier | en | fr | de | es |
| --- | --- | --- | --- | --- |
| README.md | Index | Index | Index | Índice |
| install.md | Installation | Installation | Installation | Instalación |
| configure.md | Configuration | Configuration | Konfiguration | Configuración |
| software.md | Software and services | Logiciels et services | Software und Dienste | Software y servicios |
| functions.md | Available functions | Fonctions disponibles | Verfügbare Funktionen | Funciones disponibles |
| alerts.md | Alert emails | E-mails d'alerte | Alarm-E-Mails | Correos de alerta |
| commands.md | Commands after installation | Commandes après installation | Befehle nach der Installation | Comandos después de la instalación |
| zones.md | Monitored zones | Carte des zones surveillées | Überwachte Bereiche | Mapa de zonas vigiladas |
| vpn.md | VPN | VPN | VPN | VPN |

4. Une règle `---`.
5. Une seule phrase d'ouverture : à quoi sert cette page.
6. Le corps.

## Aération

Le défaut du pack précédent est de coller trop de faits avec `<br/>`, ou de tout mettre dans un grand tableau.

Règles :

- Une phrase = un fait.
- Au plus trois phrases liées par `<br/>`. Ensuite une ligne vide.
- Un bloc de code est une île. Une phrase avant, une ligne vide, le bloc, une ligne vide, une phrase après.
- Un tableau est une île. Une phrase avant. La légende est un autre titre, pas la suite du tableau.
- Pas de mur de six `<br/>`.
- Pas d'adjectif commercial. Pas de comparaison avec une version antérieure.
- Pas de secret, de jeton, de mot de passe, ni de contenu de `secrets.conf`.

Les marques `📋` et `▪️` font partie du titre, comme dans le README du produit.
`## 📋` ouvre un chapitre. `## ▪️` ouvre un fait que l'on peut lire seul.
Ne descends pas à `###`.

## Racine

`README.md` du dépôt de test reste court. Anglais.

Bloc centré :

1. `# 🛡️ Liltress Workstation 🛡️`
2. Cinq pastilles, pas six. Ubuntu 26.04 ou plus récent, profil workstation, DNS Unbound, pare-feu UFW, contrôles AIDE · ClamAV · Lynis. Pas de pastille VPN.
3. Deux phrases, et seulement deux : le poste prépare Ubuntu 26.04 ou plus récent ; il installe le résolveur local, le pare-feu et les contrôles.

Puis le bloc `bash` exact :

```bash
sudo apt-get install -y ca-certificates curl git swaks >/dev/null 2>&1
curl -fsSL https://raw.githubusercontent.com/palarchsys/liltress-workstation/main/script/install.sh | sudo bash
```

Puis `## 📋 Documentation`.
Puis, centré : `Click a flag to open all the information.`
Puis le tableau des quatre drapeaux. Le drapeau est le lien. Le nom de la langue est sous l'image. Ordre : en, fr, de, es.
Les SVG sont ceux de `palarchsys/liltress-workstation`, `doc/flags/{en,fr,de,es}.svg`, copiés dans `doc/flags/`. Largeur 90.

La racine ne parle pas de `secrets.conf`, ne montre pas le clone, et n'explique pas `[O/N]`. Cela vit dans `install.md`.

## Index de langue

`doc/<lang>/README.md` reprend le bouclier et les cinq pastilles.
Sous les pastilles, quatre phrases liées par `<br/>`, puis une ligne vide, puis deux phrases :

- Le compte Ubuntu déjà créé n'est pas remplacé.
- L'installeur ne crée pas un second compte.
- L'arbre installé est `/opt/liltress-workstation`.
- `secrets.conf` reste sur la machine. Ne le copiez pas dans un message, un ticket ou un dépôt.

Puis : ouvrez un sujet dans la navigation ci-dessous. Le même menu est sur chaque page.

Pas d'autre corps. Le menu est dessous, pas dessus.

## Détail par page

Lis les faits dans le dépôt source avant d'écrire. Contrat des commandes : `system/sudoers/commands.csv`.
Colonnes utiles : `command`, `summary`, `detail`, `success`, `source`, `installed_path`, `workdir`.
Trente-cinq lignes. Toutes figurent dans `functions.md`. Aucune ligne inventée.
Dans le bloc `bash`, retire seulement les guillemets CSV autour de `./liltress`. Les espaces réservés restent ceux du CSV : `<marque>`, `<nom>`, `<dossier ou fichier>`, `<chemin>`, `<journal>`, `<id>`.

`doc/<lang>/vpn.md` du dépôt source reste la source du tunnel : marques, killswitch, profil de démarrage, `REPLACE`, `auth.txt`, `203.0.113.10:51820`, `10.8.0.2/24`.
Les commandes `vpn default`, `vpn default <name>` et `vpn default off` sont dans ce fichier source. Elles ne sont pas dans le CSV. Documente-les dans `vpn.md`, pas comme des lignes du CSV.

### install.md

`## 📋` Avant de commencer.
Puis `## ▪️` Système, Accès, Besoins externes.

Système : Ubuntu 26.04 ou plus récent, y compris une version intermédiaire. Une installation fraîche est préférable. Console locale ou session graphique déjà ouverte. Internet requis.

Accès : `sudo` pour `./liltress configure`, `./liltress run` et les contrôles.

Besoins externes, tableau : jeton Ubuntu Pro (obligatoire, lettres et chiffres, 6 à 100, sans espace, [https://ubuntu.com/pro/dashboard](https://ubuntu.com/pro/dashboard)) ; mot de passe d'application seulement si les alertes sont activées (8 à 128, jamais le mot de passe du compte, les espaces saisis sont retirés) ; SMTP `host:port` seulement si les alertes sont activées, exemple `smtp-mail.outlook.com:587`, crochets refusés, `swaks` envoie un essai.

`## 📋` Curl. Le bloc bash de la racine. Puis les cinq faits, par groupes de trois au plus : `install.sh` demande `[O/N]` ; `O` remplace l'arbre par la branche récupérée ; `O` lance `script/run.sh` si `script/configure.sh --check` réussit ; toute autre réponse s'arrête ; un contrôle échoué lance `script/configure.sh`.

`## 📋` Clone. Bloc :

```bash
sudo git clone https://github.com/palarchsys/liltress-workstation /opt/liltress-workstation
cd /opt/liltress-workstation
sudo ./liltress configure
sudo bash /opt/liltress-workstation/script/version-update.sh
sudo ./liltress verify boot
```

Puis : les commandes suivantes partent de `/opt/liltress-workstation`.

### configure.md

Une phrase : `sudo ./liltress configure` pose les questions, écrit `global.conf` en mode `644` et `secrets.conf` en mode `600` en root, puis lance `script/run.sh`.

Faits séparés : la première question choisit la langue ; la valeur est `LANGUAGE` ; le défaut est l'anglais ; `MODE_TEST` se répond par `0` ou `1` ; le défaut livré est `1` ; une option inconnue est refusée.

Tableau des questions. Les libellés `Login`, `Password` et `From` ne se traduisent pas. `Y` ou `y` active le courrier. `N`, `n` ou `non` le coupe. Toute autre réponse affiche `Répondez Y ou N.` Désactivé n'installe pas Postfix. Le mot de passe refuse les guillemets, `$`, `\` et les apostrophes inverses.

Si les alertes sont actives, `swaks` doit recevoir `250`. Succès : `Essai d'envoi accepté.`
Si elles restent coupées : `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

`## 📋` Contrôle sans réécriture. Commande `sudo ./liltress check`. Ne réécrit pas les fichiers. `secrets.conf` est en `600`, donc `sudo` est nécessaire pour le lire. Code `0` : les deux fichiers sont conformes. Succès : `Configuration conforme`. C'est `script/configure.sh --check`.

### software.md

Tableau à trois colonnes : nom, rôle, ce que le poste change.
Une ligne par paquet ou groupe réellement installé. Pas de second paragraphe dans une cellule : une cellule = un rôle, une cellule = les réglages.

Faits à garder dans la colonne des réglages :

- openssh-server : root refusé, `PasswordAuthentication yes`, `AllowTcpForwarding no`, `AllowAgentForwarding no`.
- ufw : entrant refusé, règle SSH explicite.
- unbound : `127.0.0.1` port 53. Le DNS d'un profil VPN n'est pas utilisé.
- usbguard et usbguard-notifier : déjà branchés, claviers, souris et hubs autorisés. Un stockage nouveau attend une confirmation du bureau.
- fail2ban : échecs d'authentification répétés bannis.
- auditd : règles pour les chemins choisis.
- aide : base `/var/lib/aide/aide.db`, reconstruction `./liltress maintain aide`.
- rkhunter : `./liltress maintain rkhunter`. `/dev/shm` et le socket `/dev/log` sont autorisés.
- chkrootkit : binaire `/usr/sbin/chkrootkit`.
- clamav : accès, démarrage, et une passe périodique chaque heure. Un fichier détecté peut partir en quarantaine.
- crowdsec : décisions locales à partir des acquisitions livrées.
- debsums : un paquet encore signalé est réinstallé avec `apt-get install --reinstall`.
- wireguard-tools, openvpn, conntrack : éteint tant qu'un profil n'est pas valide. SSH ne passe pas dans le tunnel.
- apparmor : `service/apparmor/userns.sh` retire un profil hfu qui n'est pas livré.
- unattended-upgrades : origines de sécurité. `Automatic-Reboot` vaut `false`.
- postfix : seulement si `MAIL_ALERTS=1`.
- lynis : `LYNIS_MIN_SCORE` vaut 80. `verify workstation` accepte un `hardening_index` supérieur ou égal à ce seuil.
- ubuntu-pro-client : attache le jeton. Hors LTS, pas d'esm-infra, d'esm-apps ni de Livepatch. Sur LTS, un échec de ces activations n'arrête pas l'installation.
- acct, sysstat, libpam-pwquality.
- rng-tools-debian, haveged.
- apt-listchanges, needrestart, apt-show-versions. Le courrier de redémarrage ne redémarre pas la machine.

Après le tableau, un `## ▪️` Ce qui ne change pas : pas de coupure globale d'IPv6, pas de `/tmp` en `noexec`, pas de SSH par clé seule, pas de swap recréé. Ce sont des réglages de serveur.

Une phrase : un fichier de service n'est pas une seconde copie d'une variable. Les valeurs vivent dans `global.conf`, `user.conf` ou `secrets.conf`.

### functions.md

Pas de tableau géant. Un `## 📋` par famille : lanceur, VPN, contrôles, maintenance.
Sous chaque famille, un `## ▪️` par ligne du CSV.

Chaque bloc a exactement cette forme :

```markdown
## ▪️ <nom court>

<résumé, une phrase>

​```bash
<commande>
​```

<détail, trois phrases au plus, liées par <br/> si elles vont ensemble>

<ligne vide>

Succès : `<ligne exacte>`

Source : `<chemin source>`
```

Si `success` est vide, n'invente pas de ligne de succès. Écris ce que le CSV dit du résultat (code de sortie, dossier créé, rien n'est affiché).
Si `installed_path` est rempli, ajoute `Installé : \`<chemin>\``.
Si deux lignes de succès sont séparées par ` | ` dans le CSV, montre les deux, sans traduire.

Le courrier `mirrors.dat modifié hors rkhunter --update` ne joint pas `maintain rkhunter`.
`maintain debsums` n'est pas proposé par le courrier debsums.
`verify chkrootkit` n'écrit pas la liste d'ignorés.
`scan-skip add` refuse `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, le home entier, Téléchargements, Bureau, et leur parent.
Déjà présent : `Ce chemin est déjà exclu : <path>`.
Absent au retrait : `Ce chemin n'est pas dans la liste : <path>`, sortie `1`.
Refus : `Chemin refusé : <path>`. Le fichier ne change pas.

Après la famille VPN, une phrase lien vers [vpn.md](vpn.md) pour les marques, le killswitch et le profil de démarrage.

### commands.md

Page courte pour le quotidien. Une phrase : lance ces commandes depuis `/opt/liltress-workstation`.
Un `## ▪️` par geste utile, pas toute la famille VPN :

- `sudo ./liltress check` — `Configuration conforme`
- `sudo ./liltress run` — refuse de démarrer si le contrôle échoue
- `sudo ./liltress verify alerts` et `--no-send` — sujet finit par `Contrôle`, succès `Résultat : OK`
- `sudo ./liltress verify workstation` — `Check Success : N`, `Check Warnings : N`, `Check Fails : N` ; contrôle aussi le relais SSH, la règle SSH, USBGuard, le délai sudo, `w-reboot-notice.timer`
- `sudo ./liltress verify boot` — `Contrôle de démarrage : OK` ; `en cours` ou `écarts`, sortie `1`
- `sudo ./liltress lynis` — `lynis audit system --quick --no-colors`, rapport `<source>/log/lynis-<stamp>/`, `LYNIS_MIN_SCORE` vaut 80, dernière ligne commence par `Rapport :`
- `sudo ./liltress maintain aide` — `Base de référence AIDE enregistrée.`
- `sudo ./liltress maintain rkhunter` — `Base de propriétés rkhunter enregistrée.`
- `sudo ./liltress maintain mirrors save` et `mirrors check` — fichier `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256` mode `600` ; `check` n'affiche rien ; sortie `0` si égal ou si rien n'est encore enregistré
- `scan-skip list`, `add`, `del` avec l'exemple `$HOME/.cache/exemple` et le tableau des lignes imprimées

Renvoie vers [functions.md](functions.md) pour le reste.

### alerts.md

Introduction, trois phrases au plus : chaque fichier est sous `/opt/liltress-workstation/cron/alerts` ; le courrier suit `LANGUAGE` ; il part seulement si `MAIL_ALERTS=1`, `WATCHDOG_MAIL` est défini, et `/opt/liltress-workstation/service/cron/watchdogs/send.sh` est exécutable.

Puis, séparés : un contrôle propre n'envoie rien ; une ligne de journal inconnue : ne lance pas la commande, garde le courrier ; un contrôle arrêté avant la fin envoie un courrier qui nomme le contrôle et le signal ou le code de sortie.

Un `## ▪️` par famille : AIDE, chkrootkit, debsums, ClamAV à l'accès, scan de démarrage et scan périodique, rkhunter, avis de redémarrage.

Pour chacun : ce que le message contient, la forme du journal, le bloc bash s'il y a une commande, l'effet, la ligne de succès.
Pas de commande dans le courrier debsums, ni dans l'avis de redémarrage.

Garder ces lignes exactes :

- `Base de référence AIDE enregistrée.`
- `Aucune nouvelle détection à enregistrer.`
- `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`
- la même forme avec `debsums.local.ignore`
- `La somme correspond.`
- `fichier remis en place`
- `Base de propriétés rkhunter enregistrée.`
- `Un redémarrage est utile. La machine n'a pas été redémarrée.`

Journaux : `cron/security_logs/aide-YYYYMMDD-HHMMSS.log`, et les mêmes formes pour chkrootkit, clamav, rkhunter, reboot-notice.
Quarantaine ClamAV : `sudo bash /opt/liltress-workstation/core/quarantine/restore.sh <id>`.
Registre cité par le courrier d'accès : `/var/lib/liltress-workstation/watchdog/quarantine.registry`.
Un fichier `.clamav-quarantine-lock.*` n'envoie pas de courrier. Le script sort `0` avant le journal et avant `log_alert`.
La chaîne périodique tourne toutes les 1 heure. La même passe tourne une fois au démarrage. Elle parcourt aussi les chemins exclus de l'accès.
Un fichier illisible sans ligne `FOUND` n'envoie pas de courrier. `Known viruses` et `Infected files` ne sont pas copiés dans le courrier.
Deux arrêts `clamscan` de suite arrêtent la passe. Le courrier dit que les fichiers restants n'ont pas été lus.
`/dev/shm` et `/dev/log` ne produisent pas `Suspicious file types found in /dev`.
La ligne `/var/lib/rkhunter/db/mirrors.dat` est retirée avant la réinstallation debsums seulement si son SHA-256 égale `cron/rkhunter-mirrors.sha256`.
Une ligne changée sous `/usr/lib/*/perl-base/unicore/` du paquet `perl-base` est retirée avant cette réinstallation.
Le même jeu de paquets ne renvoie pas l'avis de redémarrage. Le script ne redémarre pas la machine.

### zones.md

Une phrase : les couleurs aident la lecture. Elles ne sont pas une seconde politique. Les outils de la ligne sont la politique.

Tableau. Niveaux avec pastille thermique, une seule fois, dans la colonne du niveau :

| Zone | Niveau | Outils | Notes |
| --- | --- | --- | --- |
| `/home` hors exclusions livrées | 🔴 Haute | ClamAV à l'accès, ClamAV périodique, AIDE | Fichiers utilisateur. |
| `~/.cache`, `~/snap` | ⚪ Exclu | exclusion à l'accès | Livré dans `config/service/clamav/scan-skip.paths`. Le scan périodique passe encore dessus. |
| Téléchargements, Bureau | 🔴 Haute | ClamAV à l'accès | `scan-skip add` refuse ces chemins, leur parent, `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root` et le home entier. |
| `/etc` | 🟡 Standard | AIDE, auditd | Configuration. |
| `/opt/liltress-workstation` | 🟠 Moyen | AIDE, journaux locaux | Arbre du produit, copies d'alertes, journaux de sécurité. |
| `/var/log` | 🟢 Léger | rotation, auditd | Les motifs chkrootkit ignorent `/var/log`. |
| Stockage USB | 🟡 Standard | usbguard | Confirmation du bureau pour un stockage nouveau. |
| Fichiers empaquetés sous `/usr` | 🟢 Léger | debsums, rkhunter | Une différence debsums peut réinstaller le paquet. |

`## 📋` Légende, une puce par niveau.
Puis une phrase : changer un niveau, c'est changer `scan-skip`, AIDE, les watches auditd ou usbguard. Pas la couleur de ce tableau.

Traduis les mots du niveau. Ne traduis pas les chemins.

### vpn.md

Découpe. Ne fais pas un chapitre unique.

- `## 📋` Aucun tunnel à l'installation. Le réseau reste direct.
- `## ▪️` Importer. Les deux commandes. La marque est obligatoire. Le premier profil valide devient le VPN de démarrage et arme le killswitch. Un lot invalide n'est pas ajouté. Un import plus tard, mode coupé, ne remplace pas le VPN de démarrage et ne réarme pas.
- `## ▪️` Démarrage. `vpn default`, `vpn default <name>`, `vpn default off`. Au boot, un profil de démarrage valide arme le killswitch puis ouvre ce profil.
- `## ▪️` Mode. `vpn mode on` et `vpn mode off`. SSH reste hors du tunnel. Le profil apparaît dans Paramètres, Réseau. L'interrupteur est le mode. L'import n'est pas le bouton `+`.
- `## ▪️` Bascule. `vpn switch <name>`. Le killswitch reste armé. Le profil de démarrage ne change pas. Un échec ne rend pas Internet direct et ne retire pas le profil. Trois essais de dix secondes, après une réponse antérieure, retirent le profil. Si c'était le dernier, le réseau redevient direct, le killswitch est désarmé, et le courrier part avant ce désarmement.
- `## ▪️` Retrait et essai. `vpn rm`, `vpn test`, `vpn list`, `vpn status`.
- `## ▪️` Fichiers. `vpn/wireguard/<nom>/`, `vpn/openvpn/<nom>/`, fichier `meta`, registre `vpn/vpn.json`, journal `log/vpn/YYYY-MM-DD.log`.
- `## ▪️` Contournement. `vpn bypass`, `vpn bypass reload`, `vpn refresh-endpoint <name>`. Fichier `vpn/bypass.cidrs`. Exemple `[192.168.1.0/24, 200.200.200.0/24]`. Unbound est relancé après un changement de mode ou une bascule. L'IPv6 global est coupé seulement si le tunnel n'a pas d'IPv6.
- `## 📋` Forme d'import. Noms de fichiers de la marque, rien de plus. `auth.txt` peut être à côté du dossier, deux lignes. `REPLACE` encore présent : l'import s'arrête avant la poignée de main. `203.0.113.10` n'ouvre pas de tunnel. Les scripts `up` et `down` du fournisseur ne sont pas copiés. Le DNS du fournisseur n'est pas utilisé. Le premier `remote` est le serveur ouvert.
- `## 📋` Marques. Un tableau : marque, accepté, refusé, exemple de commande. Garde les exemples exacts du `doc/en/vpn.md` source, dont `/chemin/...`. NordVPN, ExpressVPN, Surfshark, Proton VPN, CyberGhost, Private Internet Access, Mullvad, PureVPN, IPVanish, Windscribe, Perso.
  Perso : extrémité `203.0.113.10:51820`, adresse `10.8.0.2/24`.
  Surfshark, Proton VPN, PureVPN et Windscribe acceptent les deux protocoles, pas les deux fichiers dans un même import.

## Interdit

- Inventer une commande, un chemin, une variable, un timer ou une ligne de succès.
- Copier une politique serveur : IPv6 coupé partout, `/tmp` `noexec`, SSH par clé seule, swap recréé, `LYNIS_MIN_SCORE` 90, compte `MAIN_USER`.
- Mélanger les langues dans une page.
- Lien mort.
- Emoji hors du bouclier, des cinq pastilles, des marques `📋` `▪️`, et de la colonne thermique de `zones.md`.

## Ordre

1. Racine.
2. `doc/en/*`, en relisant le CSV et `doc/en/vpn.md` source.
3. Traductions fidèles.
4. Vérifier : neuf entrées de menu, langue courante en gras, cinq pastilles, aucun mur de plus de trois `<br/>`, les trente-cinq identifiants du CSV présents dans les quatre `functions.md`, les lignes de succès identiques dans les quatre langues.

Pousse sur `main` de `palarchsys/liltress-readme-test`. Garde ce prompt dans le dépôt sous `prompt-readme-detail.md`. Ne supprime pas `prompt-readme-full.md`.
