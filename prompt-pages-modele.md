# Modèle de prompt — pages

Un humain ne modifie que les blocs `PARAMÈTRE`.
Les blocs `STANDARD` s'appliquent à toutes les pages. On ne les réécrit pas pour une page.

Trois blocs, dans cet ordre, et rien d'autre :

1. `STANDARD` — métriques de clarté, d'esthétique et d'aération.
2. `PARAMÈTRE` — navbar.
3. `PARAMÈTRE` — pages.

La langue source est l'anglais. Les autres langues traduisent le même plan, dans le même ordre.
Ne jamais traduire une commande, un chemin, un nom de variable, un nom de fichier, ni une ligne de succès.

---

# BLOC STANDARD — MÉTRIQUES

Ces chiffres valent pour chaque page. Les changer ici change tout le pack.
Une page ne peut pas assouplir une métrique.

## Clarté

| Métrique | Seuil | Comment mesurer |
| --- | --- | --- |
| Faits par phrase | 1 | Une phrase qui contient « et » entre deux faits est coupée. |
| Sujet par page | 1 | Le nom du fichier est le sujet. Un second sujet devient une autre page. |
| Phrase d'ouverture | 1 | La première phrase sous `---` dit à quoi sert la page. Pas de second paragraphe d'introduction. |
| Commande | bloc `bash` | Jamais une commande seule au milieu d'une phrase. |
| Ligne de succès | entre backticks, copiée | Si la source n'en donne pas, ne pas en écrire. |
| Source du fait | citée dans la fiche page | Un fait sans source dans la fiche est interdit. |
| Adjectif | 0 | Pas de slogan, pas de comparaison avec une version antérieure. |
| Secret | 0 | Pas de jeton, mot de passe, clé, ni contenu de `secrets.conf`. |

## Esthétique

| Métrique | Seuil | Comment mesurer |
| --- | --- | --- |
| Titre | `# 🛡️ <titre> 🛡️` centré | Le titre est celui de la navbar pour cette page. |
| Pastilles | 5, seulement racine et index | Pas de sixième pastille. Les autres pages n'ont pas de pastille. |
| Chapitre | `## 📋` | Un chapitre regroupe des faits du même geste. |
| Fait isolé | `## ▪️` | Un fait que l'on peut lire seul. Pas de `###`. |
| Menu | identique sur chaque page | Voir le bloc navbar. La page courante est en gras, sans lien. |
| Langue | une ligne sous le titre | Langue courante en gras, sans lien. Les autres sont des liens `../<code>/<fichier>`. |
| Drapeaux | SVG source, largeur 90 | Le drapeau est le lien. Le nom de la langue est sous l'image. |
| Emoji | liste fermée | Bouclier, pastilles, `📋`, `▪️`, et la colonne thermique de la page zones. Rien d'autre. |

Ordre fixe du haut de page, avant le corps :

1. Titre centré.
2. Ligne de langue.
3. Navbar.
4. `---`.
5. Phrase d'ouverture.
6. Corps de la fiche page.

## Aération

| Métrique | Seuil | Comment mesurer |
| --- | --- | --- |
| Faits liés par `<br/>` | 3 maximum | Au quatrième fait, ligne vide, nouveau groupe. |
| Après un groupe | 1 ligne vide | Pas deux lignes vides. Pas zéro. |
| Bloc de code | île | Une phrase, ligne vide, le bloc, ligne vide, une phrase. |
| Tableau | île | Une phrase avant. La légende est un autre titre, pas la suite du tableau. |
| Commandes d'une famille | 1 `▪️` par commande | Interdit : un tableau qui liste toutes les commandes. |
| Racine | 2 phrases | Pas le clone. Pas `[O/N]`. Pas `secrets.conf`. |
| Mur | 0 | Un paragraphe de plus de trois `<br/>` est un échec. |

## Contrôle avant de rendre

Cocher chaque ligne. Une case vide = la page n'est pas rendue.

- [ ] Chaque phrase porte un seul fait.
- [ ] Aucun groupe ne dépasse trois `<br/>`.
- [ ] Chaque page a une phrase d'ouverture, puis des `📋` et des `▪️`.
- [ ] La navbar a le même nombre de lignes sur chaque fichier.
- [ ] La page courante est en gras et n'est pas un lien.
- [ ] Les cinq pastilles ne sont que sur la racine et les index.
- [ ] Les lignes de succès sont identiques dans toutes les langues.
- [ ] Aucun fait ne vient d'ailleurs que de la source écrite dans la fiche.

---

# BLOC PARAMÈTRE — NAVBAR

Modifier ce tableau pour ajouter, retirer ou renommer une page.
L'ordre des lignes est l'ordre du menu.
Le fichier `README.md` de la langue est l'index. Il reste la première ligne.

| Ordre | Fichier | en | fr | de | es | Dans le menu |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | README.md | Index | Index | Index | Índice | oui |
| 2 | install.md | Installation | Installation | Installation | Instalación | oui |
| 3 | configure.md | Configuration | Configuration | Konfiguration | Configuración | oui |
| 4 | software.md | Software and services | Logiciels et services | Software und Dienste | Software y servicios | oui |
| 5 | functions.md | Available functions | Fonctions disponibles | Verfügbare Funktionen | Funciones disponibles | oui |
| 6 | alerts.md | Alert emails | E-mails d'alerte | Alarm-E-Mails | Correos de alerta | oui |
| 7 | commands.md | Commands after installation | Commandes après installation | Befehle nach der Installation | Comandos después de la instalación | oui |
| 8 | zones.md | Monitored zones | Carte des zones surveillées | Überwachte Bereiche | Mapa de zonas vigiladas | oui |
| 9 | vpn.md | VPN | VPN | VPN | VPN | oui |

Règle de rendu, non négociable :

```markdown
## Navigation

- [Installation](install.md)
- **Configuration**
```

La ligne de la page courante n'a pas de lien. Les autres en ont un, vers le fichier de la même langue.
Ajouter une page = une ligne dans ce tableau + une fiche dans le bloc pages.
Retirer une page = `Dans le menu` passe à `non`, et la fiche reste ou est retirée. Un `non` n'apparaît pas dans le menu.

---

# BLOC PARAMÈTRE — PAGES

Chaque fiche tient sur une carte. Pour une nouvelle page, copier le gabarit du bas.
Ne pas recopier ici les métriques. Elles sont déjà dans le bloc standard.

Champs d'une fiche, toujours dans cet ordre :

| Champ | Ce qu'on y met |
| --- | --- |
| Fichier | Nom, identique dans chaque langue. |
| Menu | `oui` ou `non`. Doit suivre la navbar. |
| Ouverture | Une phrase. Elle sera traduite. |
| Chapitres | Liste `📋` puis `▪️`. L'ordre est l'ordre de la page. |
| Faits | Une puce = un fait. Pas de paragraphe. |
| Ne pas dire | Ce qui serait facile à inventer, et qui est faux. |
| Source | Fichiers du dépôt à lire. Pas de fait hors de cette liste. |

## Réglages communs du pack

| Clé | Valeur |
| --- | --- |
| Produit | Liltress Workstation |
| Dépôt des faits | `palarchsys/liltress-workstation` |
| Dépôt du pack | `palarchsys/liltress-readme-test` |
| Langue source | `en` |
| Langues | `en`, `fr`, `de`, `es` |
| Arbre installé | `/opt/liltress-workstation` |
| Curl | `sudo apt-get install -y ca-certificates curl git swaks >/dev/null 2>&1` puis `curl -fsSL https://raw.githubusercontent.com/palarchsys/liltress-workstation/main/script/install.sh \| sudo bash` |
| Pastilles | Ubuntu 26.04+, workstation, Unbound, UFW, AIDE · ClamAV · Lynis |
| Drapeaux | `doc/flags/{en,fr,de,es}.svg` du dépôt des faits |
| Contrat des commandes | `system/sudoers/commands.csv` |

Ne pas dire, pour tout le pack : IPv6 coupé partout, `/tmp` en `noexec`, SSH par clé seule, swap recréé, score Lynis de serveur, compte `MAIN_USER`.

---

## Fiche — README.md racine

| Champ | Valeur |
| --- | --- |
| Fichier | `README.md` à la racine du dépôt du pack. Anglais seulement. |
| Menu | non. Les drapeaux remplacent le menu. |
| Ouverture | Deux phrases : le poste prépare Ubuntu 26.04 ou plus récent. Il installe le résolveur local, le pare-feu et les contrôles. |
| Chapitres | Titre bouclier. Cinq pastilles. Les deux phrases. Le curl. `📋 Documentation`. La phrase `Click a flag to open all the information.` Le tableau des drapeaux, ordre `en`, `fr`, `de`, `es`. |
| Faits | Rien d'autre que les deux phrases et le curl. |
| Ne pas dire | `secrets.conf`. Le clone. Le dialogue `[O/N]`. |
| Source | `README.md` du dépôt des faits. `agents/laws/readme.md` du dépôt mère pour la forme. |

## Fiche — README.md d'une langue

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/README.md` |
| Menu | oui. Titre de la navbar : Index. |
| Ouverture | Ouvrez un sujet dans la navigation ci-dessous. Le même menu est sur chaque page. |
| Chapitres | Bouclier. Cinq pastilles. Puis les phrases ci-dessous, par groupes de deux. Pas d'autre corps. |
| Faits | Le poste prépare Ubuntu 26.04 ou plus récent. Il installe le résolveur, le pare-feu et les contrôles. Le compte Ubuntu déjà créé n'est pas remplacé. L'installeur ne crée pas un second compte. L'arbre est `/opt/liltress-workstation`. `secrets.conf` reste sur la machine et ne se copie ni dans un message, ni dans un ticket, ni dans un dépôt. |
| Ne pas dire | Une liste de commandes. Un second titre après le menu. |
| Source | Même phrase de compte que `doc/en/README.md` du produit. |

## Fiche — install.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/install.md` |
| Menu | oui |
| Ouverture | Cette page est l'installation : la machine, la commande curl, et le clone. |
| Chapitres | `📋` Avant de commencer. `▪️` Système. `▪️` Accès. `▪️` Besoins externes. `📋` Curl. `📋` Clone. |
| Faits | Ubuntu 26.04 ou plus récent, y compris une version intermédiaire. Installation fraîche préférable. Console locale ou session graphique déjà ouverte. Internet requis. `sudo` pour `./liltress configure`, `./liltress run` et les contrôles. Jeton Ubuntu Pro obligatoire, lettres et chiffres, 6 à 100, sans espace. Mot de passe d'application seulement si les alertes sont actives, 8 à 128, jamais le mot de passe du compte. SMTP `host:port` seulement si les alertes sont actives, exemple `smtp-mail.outlook.com:587`, crochets refusés. `install.sh` demande `[O/N]`. `O` remplace l'arbre puis lance `script/run.sh` si `script/configure.sh --check` réussit. Toute autre réponse s'arrête. Un contrôle échoué lance `script/configure.sh`. Le clone est `git clone` vers `/opt/liltress-workstation`, puis `sudo ./liltress configure`, `script/version-update.sh`, `sudo ./liltress verify boot`. Les commandes suivantes partent de cet arbre. |
| Ne pas dire | Un second compte. Une clé SSH à la place du mot de passe. |
| Source | `script/install.sh`, `script/configure.sh`, `doc/<lang>/README.md` du produit. |

## Fiche — configure.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/configure.md` |
| Menu | oui |
| Ouverture | Cette page est ce que `sudo ./liltress configure` demande, et le contrôle qui ne réécrit pas les fichiers. |
| Chapitres | `📋` Questions. `📋` Contrôle sans réécriture. |
| Faits | Écrit `global.conf` en `644` et `secrets.conf` en `600`, en root, puis lance `script/run.sh`. La première question est la langue, valeur `LANGUAGE`, défaut anglais. `MODE_TEST` vaut `0` ou `1`, défaut livré `1`. Une option inconnue est refusée. `Y` ou `y` active le courrier. `N`, `n` ou `non` le coupe et n'installe pas Postfix. Toute autre réponse affiche `Répondez Y ou N.` Les libellés `Login`, `Password`, `From` ne se traduisent pas. Le mot de passe refuse guillemets, `$`, `\`, apostrophes inverses. `swaks` doit recevoir `250`. Succès : `Essai d'envoi accepté.` Alertes coupées : `Alertes e-mail coupées. Les contrôles partiront sans envoi.` `sudo ./liltress check` ne réécrit rien. Succès : `Configuration conforme`. C'est `script/configure.sh --check`. |
| Ne pas dire | Un exemple de jeton ou de mot de passe. |
| Source | `script/configure.sh`. |

## Fiche — software.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/software.md` |
| Menu | oui |
| Ouverture | Cette page est les paquets installés, et ce que le poste change. |
| Chapitres | `📋` Tableau. `▪️` Ce qui ne change pas. |
| Faits | Un tableau, trois colonnes : nom, rôle, ce que le poste change. Une cellule = un rôle. Lignes : openssh-server (`PasswordAuthentication yes`, `AllowTcpForwarding no`, `AllowAgentForwarding no`, root refusé), ufw, unbound (`127.0.0.1` port 53), usbguard, fail2ban, auditd, aide, rkhunter, chkrootkit, clamav, crowdsec, debsums, wireguard-tools + openvpn + conntrack, apparmor, unattended-upgrades (`Automatic-Reboot` `false`), postfix seulement si `MAIL_ALERTS=1`, lynis (`LYNIS_MIN_SCORE` 80), ubuntu-pro-client, acct + sysstat + libpam-pwquality, rng-tools-debian + haveged, apt-listchanges + needrestart + apt-show-versions. Un fichier de service n'est pas une seconde copie d'une variable : les valeurs vivent dans `global.conf`, `user.conf` ou `secrets.conf`. Ce qui ne change pas : pas d'IPv6 coupé partout, pas de `/tmp` `noexec`, SSH par mot de passe, pas de swap recréé. |
| Ne pas dire | Un paquet qui n'est pas dans `service/` ou dans le README du produit. |
| Source | `doc/<lang>/README.md` du produit. `config/service/`. |

## Fiche — functions.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/functions.md` |
| Menu | oui |
| Ouverture | Chaque bloc est une ligne de `system/sudoers/commands.csv`. La ligne de succès est celle du script. Elle n'est pas traduite. |
| Chapitres | `📋` Le lanceur. `📋` VPN. `📋` Contrôles. `📋` Maintenance. Sous chaque chapitre, un `▪️` par ligne du CSV de cette famille. |
| Faits | Trente-cinq lignes. Forme d'un `▪️` : le résumé en une phrase, le bloc bash (retirer seulement les guillemets CSV autour de `./liltress`), le détail en groupes de trois faits, le répertoire de travail, la ou les lignes `success`, `Source : \`<chemin>\``. Si `installed_path` est rempli, une ligne `Installé`. Si `success` est vide, ne pas inventer de bannière. Après le chapitre VPN, un lien vers `vpn.md` pour les marques, le killswitch et le profil de démarrage. |
| Ne pas dire | `vpn default` dans cette page : cette commande est dans `vpn.md`, pas dans le CSV. |
| Source | `system/sudoers/commands.csv`. |

## Fiche — alerts.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/alerts.md` |
| Menu | oui |
| Ouverture | Cette page est ce que contient un courrier d'alerte, et la commande qui va avec. |
| Chapitres | `📋` Quand le courrier part. `▪️` AIDE. `▪️` chkrootkit. `▪️` debsums. `▪️` ClamAV à l'accès. `▪️` Scan de démarrage et scan périodique. `▪️` rkhunter. `▪️` Avis de redémarrage. |
| Faits | Fichiers sous `/opt/liltress-workstation/cron/alerts`. Le courrier suit `LANGUAGE`. Il part seulement si `MAIL_ALERTS=1`, `WATCHDOG_MAIL` est défini, et `service/cron/watchdogs/send.sh` est exécutable. Un contrôle propre n'envoie rien. Une ligne de journal inconnue : ne pas lancer la commande, garder le courrier. Succès à garder tels quels : `Base de référence AIDE enregistrée.` `Aucune nouvelle détection à enregistrer.` `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore` et la même forme pour `debsums.local.ignore`. `La somme correspond.` `fichier remis en place`. `Base de propriétés rkhunter enregistrée.` `Un redémarrage est utile. La machine n'a pas été redémarrée.` |
| Ne pas dire | Une commande dans le courrier debsums. Une commande dans l'avis de redémarrage. Un redémarrage lancé par le script. |
| Source | `doc/<lang>/README.md` du produit, section des alertes. `service/cron/watchdogs/`. |

## Fiche — commands.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/commands.md` |
| Menu | oui |
| Ouverture | Ces commandes se lancent depuis `/opt/liltress-workstation`. |
| Chapitres | Un `▪️` par geste quotidien : check, run, verify alerts, verify workstation, verify boot, lynis, maintain aide, maintain rkhunter, mirrors, scan-skip. Pas la famille VPN. |
| Faits | Les lignes de succès de ces gestes, copiées du CSV. `LYNIS_MIN_SCORE` vaut 80. L'exemple d'exclusion est `$HOME/.cache/exemple`. Renvoyer vers `functions.md` pour le reste. |
| Ne pas dire | Le tableau des marques VPN. |
| Source | `system/sudoers/commands.csv`. |

## Fiche — zones.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/zones.md` |
| Menu | oui |
| Ouverture | Les couleurs aident la lecture. Elles ne sont pas une seconde politique. Les outils de la ligne sont la politique. |
| Chapitres | `📋` Tableau. `📋` Légende. |
| Faits | Colonnes : zone, niveau, outils, notes. Niveaux : `/home` hors exclusions livrées 🔴 haute (ClamAV à l'accès, ClamAV périodique, AIDE). `~/.cache` et `~/snap` ⚪ exclu, fichier `config/service/clamav/scan-skip.paths`, le scan périodique passe encore dessus. Téléchargements et Bureau 🔴 haute, `scan-skip add` les refuse ainsi que `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root` et le home entier. `/etc` 🟡 standard (AIDE, auditd). `/opt/liltress-workstation` 🟠 moyen (AIDE, journaux locaux). `/var/log` 🟢 léger (rotation, auditd). Stockage USB 🟡 standard (usbguard). Fichiers empaquetés sous `/usr` 🟢 léger (debsums, rkhunter). Changer un niveau, c'est changer l'outil, pas la couleur du tableau. |
| Ne pas dire | Une zone qui n'est pas dans cette liste. |
| Source | `config/service/clamav/scan-skip.paths`. `config/service/integrity/`. `config/service/auditd/`. |

## Fiche — vpn.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/vpn.md` |
| Menu | oui |
| Ouverture | L'installation démarre sans VPN. Le réseau reste direct. |
| Chapitres | `▪️` Import. `▪️` Profil de démarrage. `▪️` Mode. `▪️` Bascule. `▪️` Retrait et essai. `▪️` Fichiers. `▪️` Contournement. `📋` Forme d'import. `📋` Marques. |
| Faits | La marque est obligatoire. Le premier profil valide arme le killswitch et devient le VPN de démarrage. `vpn default`, `vpn default <name>`, `vpn default off`. `vpn mode on`, `vpn mode off`. SSH hors du tunnel. `vpn switch` ne change pas le profil de démarrage. Trois essais de dix secondes retirent un profil qui avait déjà répondu. Le courrier part avant le désarmement du dernier profil. Chemins `vpn/wireguard/<nom>/`, `vpn/openvpn/<nom>/`, `meta`, `vpn/vpn.json`, `log/vpn/YYYY-MM-DD.log`, `vpn/bypass.cidrs`. `REPLACE` encore présent : l'import s'arrête. `203.0.113.10` n'ouvre pas de tunnel. Perso : `203.0.113.10:51820` et `10.8.0.2/24`. Unbound reste le résolveur. |
| Ne pas dire | Une marque absente du `vpn.md` source. Un second protocole dans le même import pour Surfshark, Proton VPN, PureVPN ou Windscribe. |
| Source | `doc/en/vpn.md` du dépôt des faits. |

---

# BLOC PARAMÈTRE — GABARIT D'UNE PAGE NOUVELLE

Copier cette carte. Remplir les cases. Ajouter une ligne dans la navbar.
Ne pas écrire de métrique ici.

## Fiche — <fichier>.md

| Champ | Valeur |
| --- | --- |
| Fichier | `doc/<lang>/<fichier>.md` |
| Menu | oui |
| Ouverture | Une phrase. |
| Chapitres | `📋` …. `▪️` …. |
| Faits | |
| Ne pas dire | |
| Source | |

---

# Ordre de génération

1. Lire le bloc standard. Appliquer les seuils, sans les réinterpréter.
2. Lire la navbar. Construire le menu avant d'écrire une page.
3. Lire chaque fiche. Écrire l'anglais, puis les traductions du même plan.
4. Cocher le contrôle du bloc standard.
5. Déposer le pack sur le dépôt indiqué dans les réglages communs. Ne pas effacer les prompts déjà présents.
