<div align="center">

# 🛡️ Commandes après installation 🛡️

</div>

[English](../en/commands.md) · **Français** · [Deutsch](../de/commands.md) · [Español](../es/commands.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- [E-mails d'alerte](alerts.md)
- **Commandes après installation**
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)

---

Lancez ces commandes depuis `/opt/liltress-workstation`.

## 📋 Contrôler la configuration

```bash
sudo ./liltress check
```

Contrôle `global.conf` et `secrets.conf`.<br/>
La ligne de succès est `Configuration conforme`.

## 📋 Relancer l'installeur

```bash
sudo ./liltress run
```

Démarre depuis les fichiers préparés.<br/>
L'installeur refuse de démarrer si le contrôle ne passe pas.

## 📋 Sonder le chemin d'alerte

```bash
sudo ./liltress verify alerts
```

Contrôle le timer de démarrage, le timer périodique, ClamAV à l'accès et le chemin du courrier.<br/>
Sans `--no-send`, une sonde part lorsque les alertes sont activées.<br/>
Le sujet se termine par `Contrôle`.<br/>
La ligne de succès est `Résultat : OK`.

```bash
sudo ./liltress verify alerts --no-send
```

Même contrôle. Aucune sonde.

## 📋 Contrôler le poste

```bash
sudo ./liltress verify workstation
```

Lignes de clôture : `Check Success : N`, `Check Warnings : N`, `Check Fails : N`.<br/>
La même passe contrôle le relais SSH, la règle de pare-feu SSH, USBGuard, le délai sudo et `w-reboot-notice.timer`.

## 📋 Contrôler ce démarrage

```bash
sudo ./liltress verify boot
```

Succès : `Contrôle de démarrage : OK`.<br/>
Encore en cours : `Contrôle de démarrage : en cours`, sortie `1`.<br/>
Un écart : `Contrôle de démarrage : écarts`, sortie `1`.

## 📋 Lynis

```bash
sudo ./liltress lynis
```

Lance `lynis audit system --quick --no-colors`.<br/>
Le rapport est sous `<source>/log/lynis-<stamp>/`.<br/>
`LYNIS_MIN_SCORE` vaut 80.

## 📋 Reconstruire les bases

```bash
sudo ./liltress maintain aide
sudo ./liltress maintain rkhunter
```

Succès AIDE : `Base de référence AIDE enregistrée.`<br/>
Succès rkhunter : `Base de propriétés rkhunter enregistrée.`

## 📋 Empreinte des miroirs rkhunter

```bash
sudo ./liltress maintain mirrors save
sudo ./liltress maintain mirrors check
```

`mirrors save` écrit le SHA-256 dans `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, mode `600`.<br/>
`mirrors check` n'affiche rien. La sortie `0` signifie que les empreintes correspondent, ou qu'aucune n'est encore enregistrée.

## 📋 Chemins exclus de l'accès

```bash
sudo ./liltress maintain scan-skip list
sudo ./liltress maintain scan-skip add $HOME/.cache/exemple
sudo ./liltress maintain scan-skip del $HOME/.cache/exemple
```

| Action | Ligne affichée |
| --- | --- |
| `list` | un chemin accepté par ligne |
| `add` | `Chemin ajouté : <path>` |
| déjà présent | `Ce chemin est déjà exclu : <path>` |
| `del` | `Chemin retiré : <path>` |
| refusé | `Chemin refusé : <path>` |

La liste complète, VPN et restauration de quarantaine compris, est dans [functions.md](functions.md).
