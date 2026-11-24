# Software y servicios

[English](../en/software.md) · [Français](../fr/software.md) · [Deutsch](../de/software.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- [Configuración](configure.md)
- **Software y servicios**
- [Funciones disponibles](functions.md)
- [Correos de alerta](alerts.md)
- [Comandos después de la instalación](commands.md)
- [Mapa de zonas vigiladas](zones.md)
- [VPN](vpn.md)

La instalación coloca los paquetes y servicios de abajo. Cada uno se configura desde `config/service/<name>/` y el script correspondiente bajo `service/`.

| Nombre | Papel | Qué cambia el puesto |
| --- | --- | --- |
| openssh-server | Servidor SSH | Se rechaza el inicio de sesión de root. `PasswordAuthentication yes`. `AllowTcpForwarding no`. `AllowAgentForwarding no`. |
| ufw | Cortafuegos del host | La política de entrada es denegar. La regla SSH permanece explícita. |
| unbound | Resolutor DNS local | Escucha en `127.0.0.1`, puerto 53. El DNS del proveedor de un perfil VPN no se usa. |
| usbguard, usbguard-notifier | Política de dispositivos USB | Los dispositivos ya conectados, más teclados, ratones y concentradores, están permitidos. Un almacenamiento nuevo espera una confirmación del escritorio. |
| fail2ban | Bloqueos de autenticación | Los fallos de autenticación repetidos se bloquean. |
| auditd | Pista de auditoría | Escribe reglas para las rutas elegidas. |
| aide | Integridad de archivos | Base en `/var/lib/aide/aide.db`. Reconstrucción con `./liltress maintain aide`. |
| rkhunter | Rootkit y propiedades | La base de propiedades se refresca con `./liltress maintain rkhunter`. `/dev/shm` y el socket `/dev/log` están permitidos. |
| chkrootkit | Comprobación de rootkit | Binario `/usr/sbin/chkrootkit`. Las líneas ruidosas conocidas pueden registrarse desde un diario. |
| clamav | Análisis de malware | Análisis al acceso, análisis de arranque y un análisis periódico cada hora. Una detección puede mover el archivo a cuarentena. |
| crowdsec | Decisiones locales | Decisiones de seguridad locales a partir de las adquisiciones entregadas. |
| debsums | Sumas de los paquetes | Un paquete que sigue denunciado se reinstala con `apt-get install --reinstall`. |
| wireguard-tools, openvpn, conntrack | Un túnel | Permanece apagado hasta que un perfil es válido. SSH no entra en el túnel. |
| apparmor | Perfiles de la distribución | `service/apparmor/userns.sh` retira un perfil hfu que no se entrega. |
| unattended-upgrades | Actualizaciones de seguridad | Orígenes de seguridad. `Automatic-Reboot` es `false`. |
| postfix | Correo de alerta | Solo se instala cuando `MAIL_ALERTS=1`. |
| lynis | Auditoría de endurecimiento | `LYNIS_MIN_SCORE` es 80. `verify workstation` acepta un `hardening_index` mayor o igual a esa puntuación. |
| ubuntu-pro-client | Enlace Ubuntu Pro | Enlaza el token. Una versión que no es LTS no activa esm-infra, esm-apps ni Livepatch. En LTS, un fallo al activarlos no detiene la instalación. |
| acct, sysstat, libpam-pwquality | Contabilidad y contraseñas | Contabilidad de procesos, estadísticas de actividad, calidad de contraseña. |
| rng-tools-debian, haveged | Entropía | Fuentes de entropía adicionales. |
| apt-listchanges, needrestart, apt-show-versions | Avisos de paquetes | Avisos de cambios y comprobaciones de reinicio. El correo de aviso de reinicio no reinicia la máquina. |

Un archivo de servicio no es una segunda copia de una variable de contrato. Los valores viven en `global.conf`, `user.conf` o `secrets.conf`.
