<div align="center">

# 🛡️ Software and services 🛡️

</div>

**English** · [Français](../fr/software.md) · [Deutsch](../de/software.md) · [Español](../es/software.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- **Software and services**
- [Available functions](functions.md)
- [Alert emails](alerts.md)
- [Commands after installation](commands.md)
- [Monitored zones](zones.md)
- [VPN](vpn.md)

---

## 📋 This guide

The installation places the packages and services below. Each one is configured from `config/service/<name>/` and the matching script under `service/`.

| Name | Role | What the workstation setup changes |
| --- | --- | --- |
| openssh-server | SSH server | Root login is refused. `PasswordAuthentication yes`. `AllowTcpForwarding no`. `AllowAgentForwarding no`. |
| ufw | Host firewall | Incoming policy is deny. The SSH rule stays explicit. |
| unbound | Local DNS resolver | Listens on `127.0.0.1`, port 53. Provider DNS from a VPN profile is not used. |
| usbguard, usbguard-notifier | USB device policy | Devices already plugged in, plus keyboards, mice and hubs, are allowed. A new storage device waits for a desktop confirmation. |
| fail2ban | Authentication bans | Repeated authentication failures are banned. |
| auditd | Audit trail | Writes rules for the selected paths. |
| aide | File integrity | Database at `/var/lib/aide/aide.db`. Rebuild with `./liltress maintain aide`. |
| rkhunter | Rootkit and properties | Property database refreshed with `./liltress maintain rkhunter`. `/dev/shm` and the socket `/dev/log` are allowed. |
| chkrootkit | Rootkit check | Binary `/usr/sbin/chkrootkit`. Known noisy lines can be recorded from a journal. |
| clamav | Malware scan | On-access scan, boot scan, and a periodic scan every hour. A detection can move the file to quarantine. |
| crowdsec | Local decisions | Local security decisions from the shipped acquisitions. |
| debsums | Package checksums | A still-reported package is reinstalled with `apt-get install --reinstall`. |
| wireguard-tools, openvpn, conntrack | One tunnel | Stays off until a profile is validated. SSH does not enter the tunnel. |
| apparmor | Distribution profiles | `service/apparmor/userns.sh` removes an hfu profile that is not shipped. |
| unattended-upgrades | Security updates | Security origins. `Automatic-Reboot` is `false`. |
| postfix | Alert mail | Installed only when `MAIL_ALERTS=1`. |
| lynis | Hardening audit | `LYNIS_MIN_SCORE` is 80. `verify workstation` accepts a `hardening_index` greater than or equal to that score. |
| ubuntu-pro-client | Ubuntu Pro attach | Attaches the token. A non-LTS release does not enable esm-infra, esm-apps or Livepatch. On LTS, a failure to enable them does not stop the installation. |
| acct, sysstat, libpam-pwquality | Accounting and passwords | Process accounting, activity stats, password quality. |
| rng-tools-debian, haveged | Entropy | Extra entropy sources. |
| apt-listchanges, needrestart, apt-show-versions | Package notices | Change notices and restart checks. The reboot notice mail does not reboot the machine. |

A service file is not a second copy of a contract variable. Values live in `global.conf`, `user.conf`, or `secrets.conf`.
