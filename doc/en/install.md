<div align="center">

# 🛡️ Installation 🛡️

</div>

**English** · [Français](../fr/install.md) · [Deutsch](../de/install.md) · [Español](../es/install.md)

## Navigation

- [Index](README.md)
- **Installation**
- [Configuration](configure.md)
- [Software and services](software.md)
- [Available functions](functions.md)
- [Alert emails](alerts.md)
- [Commands after installation](commands.md)
- [Monitored zones](zones.md)
- [VPN](vpn.md)

---

## 📋 Before you start

Three points come before installation: the system, access, and the external needs.

## ▪️ System

Ubuntu 26.04 or newer, including an interim release.<br/>
A fresh installation is preferred.<br/>
Use a local console or the existing graphical session.

The machine needs Internet access.

## ▪️ Access

You need `sudo` for `./liltress configure`, `./liltress run`, and the checks.

## ▪️ External needs

| Need | Procedure |
| --- | --- |
| Ubuntu Pro token | Required. Open [https://ubuntu.com/pro/dashboard](https://ubuntu.com/pro/dashboard) and copy the token. Letters and digits only, length 6 to 100, no space. |
| Application password | Asked only if mail alerts are enabled. Use the provider application password. Never the account password. Length 8 to 128. Spaces typed in the prompt are removed. |
| SMTP server | Asked only if alerts are enabled. Form `host:port`. Example: `smtp-mail.outlook.com:587`. Brackets are refused. `swaks` sends one test message. |

## 📋 Curl

```bash
sudo apt-get install -y ca-certificates curl git swaks >/dev/null 2>&1
curl -fsSL https://raw.githubusercontent.com/palarchsys/liltress-workstation/main/script/install.sh | sudo bash
```

`install.sh` asks `[O/N]` on the terminal.<br/>
`O` replaces the installed tree with the fetched branch.<br/>
`O` runs `script/run.sh` when `script/configure.sh --check` succeeds.

Any other answer stops.<br/>
A failed check runs `script/configure.sh`.

## 📋 Clone

```bash
sudo git clone https://github.com/palarchsys/liltress-workstation /opt/liltress-workstation
cd /opt/liltress-workstation
sudo ./liltress configure
sudo bash /opt/liltress-workstation/script/version-update.sh
sudo ./liltress verify boot
```

Later commands run from `/opt/liltress-workstation`.
