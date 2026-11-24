# Installed software and services

| Name                  | Role                                      | Improvement                          |
|-----------------------|-------------------------------------------|--------------------------------------|
| Unbound               | Local DNS resolver on 127.0.0.1:53        | No external DNS queries              |
| UFW                   | Host firewall                             | Default deny incoming                |
| ClamAV                | On-access and boot scanning               | Quarantine on detection              |
| AIDE                  | File integrity database                   | Baseline after installation          |
| rkhunter              | Rootkit and file property check           | Property database refresh            |
| chkrootkit            | Rootkit check                             | Pattern ignore list                  |
| usbguard              | USB device control                        | Confirmation for new storage         |
| fail2ban              | Ban repeated authentication failures      | Local decisions                      |
| auditd                | Audit rules for selected paths            | System events                        |
| CrowdSec              | Local security decisions                  | Collaborative intelligence           |
| debsums               | Checksums of packaged files               | Automatic reinstall on mismatch      |
| unattended-upgrades   | Security updates                          | No automatic reboot                  |
| postfix               | Alert mail                                | Only when MAIL_ALERTS=1              |
| Lynis                 | Hardening audit                           | Minimum score 80                     |

Each service is configured by its own script under `service/<name>/`.
