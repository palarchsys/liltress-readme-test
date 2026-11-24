# Installation

[English](../en/install.md) · **Français** · [Deutsch](../de/install.md) · [Español](../es/install.md)

## Navigation

- [Index](README.md)
- **Installation**
- [Configuration](configure.md)
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- [E-mails d'alerte](alerts.md)
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)

## Avant de commencer

Trois points précèdent l'installation : le système, l'accès et les besoins externes.

### Système

Ubuntu 26.04 ou plus récent, y compris une version intermédiaire.

Une installation fraîche est préférable.

Utilisez une console locale ou la session graphique existante.

La machine a besoin d'un accès Internet.

### Accès

Vous avez besoin de `sudo` pour `./liltress configure`, `./liltress run` et les contrôles.

### Besoins externes

| Besoin | Procédure |
| --- | --- |
| Jeton Ubuntu Pro | Obligatoire. Ouvrez [https://ubuntu.com/pro/dashboard](https://ubuntu.com/pro/dashboard) et copiez le jeton. Lettres et chiffres seulement, longueur 6 à 100, sans espace. |
| Mot de passe d'application | Demandé seulement si les alertes mail sont activées. Utilisez le mot de passe d'application du fournisseur. Jamais le mot de passe du compte. Longueur 8 à 128. Les espaces saisis sont retirés. |
| Serveur SMTP | Demandé seulement si les alertes sont activées. Forme `host:port`. Exemple : `smtp-mail.outlook.com:587`. Les crochets sont refusés. `swaks` envoie un message d'essai. |

## Curl

```bash
sudo apt-get install -y ca-certificates curl git swaks >/dev/null 2>&1
curl -fsSL https://raw.githubusercontent.com/palarchsys/liltress-workstation/main/script/install.sh | sudo bash
```

`install.sh` pose `[O/N]` sur le terminal.

`O` remplace l'arbre installé par la branche récupérée.

`O` lance `script/run.sh` lorsque `script/configure.sh --check` réussit.

Toute autre réponse arrête.

Un contrôle en échec lance `script/configure.sh`.

## Clone

```bash
sudo git clone https://github.com/palarchsys/liltress-workstation /opt/liltress-workstation
cd /opt/liltress-workstation
sudo ./liltress configure
sudo bash /opt/liltress-workstation/script/version-update.sh
sudo ./liltress verify boot
```

Les commandes suivantes se lancent depuis `/opt/liltress-workstation`.
