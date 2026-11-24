Génère la documentation multi-locale complète pour Liltress Workstation (dépôt source : palarchsys/liltress-workstation) destinée au dépôt de test palarchsys/liltress-readme-test.

Objectif : READMEs clairs, aérés, homogènes, 100 % compatibles GitHub, avec menu/navbar et drapeaux.

## Contraintes
- Anglais = source de vérité. Les autres langues (fr, de, es) sont des traductions fidèles du même plan, mêmes commandes, mêmes chemins.
- Respecter autant que possible agents/laws/readme.md du dépôt mère palarchsys/liltress.
- Markdown pur. Pas de HTML complexe sauf le tableau des drapeaux déjà autorisé.
- Une idée par paragraphe. Aération généreuse. Tableaux pour les listes.
- Commandes et chemins jamais traduits.

## Structure à générer

### 1. README.md (racine du dépôt de test)
- Titre centré + pastilles
- 2 phrases courtes
- Commande curl copier/coller
- Tableau des 4 drapeaux (SVG dans doc/flags/) pointant vers doc/<lang>/README.md
- Phrase : "Click a flag to open all the information."

### 2. Système de menu / navbar
Dans chaque doc/<lang>/README.md, ajouter en haut (après le titre) un menu simple et cliquable :

```markdown
## Navigation

- [Installation](install.md)
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- [E-mails d’alerte](alerts.md)
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)
```

Le même menu doit apparaître en haut de chaque fichier de documentation pour faciliter la navigation.

### 3. Fichiers à créer pour chaque langue (en, fr, de, es)

- doc/<lang>/README.md → index + menu
- doc/<lang>/install.md
- doc/<lang>/configure.md
- doc/<lang>/software.md → tableau des services + améliorations
- doc/<lang>/functions.md → chaque commande de system/sudoers/commands.csv avec effet, fichier modifié, message de succès
- doc/<lang>/alerts.md
- doc/<lang>/commands.md
- doc/<lang>/zones.md → carte mosaïque des zones surveillées
- doc/<lang>/vpn.md (lien ou contenu existant)

### 4. Carte des zones (zones.md)
Tableau Markdown avec niveaux :

| Zone | Niveau | Outils | Notes |
|------|--------|--------|-------|
| /home | 🔴 Haute | ClamAV on-access, AIDE, rkhunter | ... |

Légende claire sous le tableau.

### 5. Drapeaux
Utiliser les SVG déjà présents dans le dépôt source (doc/flags/*.svg). Les copier ou les référencer correctement dans le README racine.

### 6. Contenu
- Détail des fonctions basé sur commands.csv (liltress-workstation)
- Services avec rôle + amélioration apportée
- Messages de succès exacts
- Aucune invention de commande ou de chemin

## Ordre de génération
1. README.md racine
2. doc/en/* (source)
3. Traductions fr, de, es
4. Vérifier la cohérence du menu dans tous les fichiers

Commence par générer l’ensemble pour en et fr, puis étends aux autres langues.
