<div align="center">

# 🛡️ Monitored zones 🛡️

</div>

**English** · [Français](../fr/zones.md) · [Deutsch](../de/zones.md) · [Español](../es/zones.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- [Configuration](configure.md)
- [Software and services](software.md)
- [Available functions](functions.md)
- [Alert emails](alerts.md)
- [Commands after installation](commands.md)
- **Monitored zones**
- [VPN](vpn.md)

---

The colours are a reading aid. They are not a second policy. The tools in the row are the policy.

| Zone | Level | Tools | Notes |
| --- | --- | --- | --- |
| `/home` except the shipped skips | 🔴 High | ClamAV on-access, periodic ClamAV, AIDE | User files. |
| `~/.cache`, `~/snap` | ⚪ Excluded | On-access skip | Shipped in `config/service/clamav/scan-skip.paths`. The periodic scan still walks paths excluded from on-access. |
| Downloads, Desktop | 🔴 High | ClamAV on-access | `scan-skip add` refuses these paths, their parent, `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, and the whole home. |
| `/etc` | 🟡 Standard | AIDE, auditd | Configuration files watched by the integrity baseline and the audit rules. |
| `/opt/liltress-workstation` | 🟠 Medium | AIDE, local logs | Product tree, alert copies, and security journals. |
| `/var/log` | 🟢 Light | log rotation, auditd | `chkrootkit` patterns ignore `/var/log`. |
| USB storage | 🟡 Standard | usbguard | A new storage device waits for a desktop confirmation. Keyboards, mice, hubs, and devices already present are allowed. |
| Packaged files under `/usr` | 🟢 Light | debsums, rkhunter | Checksums and the property database. A debsums mismatch can reinstall the package. |

## 📋 Legend

- 🔴 High: on-access scanning plus integrity or a path that cannot be excluded.
- 🟠 Medium: integrity and the product tree.
- 🟡 Standard: configuration, audit, or a device confirmation.
- 🟢 Light: checksums, properties, or logs.
- ⚪ Excluded: named in the on-access skip list. This is not a silent hole in the periodic scan.

Changing a level means changing the tool configuration (`scan-skip`, AIDE, audit watches, usbguard), not the colour in this table.
