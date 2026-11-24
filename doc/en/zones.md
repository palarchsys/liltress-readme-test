# Monitored zones

Surveillance level by zone.

| Zone              | Level     | Tools                          | Notes                          |
|-------------------|-----------|--------------------------------|--------------------------------|
| `/home`           | 🔴 High   | ClamAV on-access, AIDE, rkhunter | User files                     |
| `Downloads`       | 🔴 High   | ClamAV on-access, AIDE         | Frequent changes               |
| `Desktop`         | 🟠 Medium | ClamAV on-access               | User workspace                 |
| `/opt`            | 🟠 Medium | AIDE, rkhunter                 | Installed software             |
| `/etc`            | 🟡 Standard | AIDE, auditd                 | System configuration           |
| `/var/log`        | 🟢 Light  | log rotation, auditd           | Logs                           |
| USB devices       | 🟡 Standard | usbguard                     | Confirmation required          |
| System binaries   | 🟢 Light  | debsums, rkhunter              | Package integrity              |

## Legend

- 🔴 High : on-access scanning + integrity + rootkit
- 🟠 Medium : integrity or on-access
- 🟡 Standard : configuration or device control
- 🟢 Light : checksums or logs only
