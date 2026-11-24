# Zones surveillées

Niveau de surveillance par zone.

| Zone              | Niveau    | Outils                         | Notes                          |
|-------------------|-----------|--------------------------------|--------------------------------|
| `/home`           | 🔴 Haute  | ClamAV à l'accès, AIDE, rkhunter | Fichiers utilisateur         |
| `Téléchargements` | 🔴 Haute  | ClamAV à l'accès, AIDE         | Changements fréquents          |
| `Bureau`          | 🟠 Moyen  | ClamAV à l'accès               | Espace de travail              |
| `/opt`            | 🟠 Moyen  | AIDE, rkhunter                 | Logiciels installés            |
| `/etc`            | 🟡 Standard | AIDE, auditd                 | Configuration système          |
| `/var/log`        | 🟢 Léger  | rotation des logs, auditd      | Journaux                       |
| Périphériques USB | 🟡 Standard | usbguard                     | Confirmation requise           |
| Binaires système  | 🟢 Léger  | debsums, rkhunter              | Intégrité des paquets          |

## Légende

- 🔴 Haute : scan à l'accès + intégrité + rootkit
- 🟠 Moyen : intégrité ou scan à l'accès
- 🟡 Standard : configuration ou contrôle de périphérique
- 🟢 Léger : sommes de contrôle ou journaux uniquement
