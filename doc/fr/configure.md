<div align="center">

# 🛡️ Configuration 🛡️

</div>

[English](../en/configure.md) · **Français** · [Deutsch](../de/configure.md) · [Español](../es/configure.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- **Configuration**
- [Logiciels et services](software.md)
- [Fonctions disponibles](functions.md)
- [E-mails d'alerte](alerts.md)
- [Commandes après installation](commands.md)
- [Carte des zones surveillées](zones.md)
- [VPN](vpn.md)

---

`sudo ./liltress configure` pose les questions ci-dessous, écrit `global.conf` en mode `644` et `secrets.conf` en mode `600` en root, puis lance `script/run.sh`.<br/>
La première question choisit la langue.<br/>
La valeur est `LANGUAGE`.<br/>
La valeur par défaut est l'anglais.<br/>
`MODE_TEST` se répond par `0` ou `1`. Le défaut livré est `1`.<br/>
Une option inconnue est refusée.

| Question | Réponse attendue |
| --- | --- |
| Jeton Ubuntu Pro | Lettres et chiffres, 6 à 100, sans espace. Obligatoire. |
| Activer le courrier | `Y` ou `y` active le courrier. `N`, `n` ou `non` le coupe. Désactivé n'installe pas Postfix. Toute autre réponse affiche `Répondez Y ou N.` |
| Serveur | `host:port`, par exemple `smtp-mail.outlook.com:587`. Sans crochets. Demandé seulement si les alertes sont activées. |
| Login | Une adresse e-mail. |
| Password | Le mot de passe d'application, 8 à 128 caractères. Les espaces sont retirés. Les guillemets, `$`, `\` et les apostrophes inverses sont refusés. Jamais le mot de passe du compte. |
| From | Une adresse e-mail. `WATCHDOG_MAIL` reçoit la même adresse. |

Lorsque les alertes sont activées, `swaks` doit recevoir un `250`.<br/>
La ligne de succès de cet essai est `Essai d'envoi accepté.`<br/>
Lorsque les alertes restent coupées, la ligne est `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

## 📋 Contrôle sans réécriture

```bash
sudo ./liltress check
```

La commande contrôle `global.conf` et `secrets.conf` sans les réécrire.<br/>
`secrets.conf` est en mode `600`, donc `sudo` est requis pour le lire.<br/>
Le code `0` signifie que les deux fichiers sont conformes.<br/>
La ligne de succès est `Configuration conforme`.<br/>
C'est le même contrôle que `script/configure.sh --check`.
