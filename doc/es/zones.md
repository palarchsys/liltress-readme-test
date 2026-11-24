# Mapa de zonas vigiladas

[English](../en/zones.md) · [Français](../fr/zones.md) · [Deutsch](../de/zones.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- [Configuración](configure.md)
- [Software y servicios](software.md)
- [Funciones disponibles](functions.md)
- [Correos de alerta](alerts.md)
- [Comandos después de la instalación](commands.md)
- **Mapa de zonas vigiladas**
- [VPN](vpn.md)

Los colores ayudan a la lectura. No son una segunda política. Las herramientas de la fila son la política.

| Zona | Nivel | Herramientas | Notas |
| --- | --- | --- | --- |
| `/home` salvo las exclusiones entregadas | 🔴 Alta | ClamAV al acceso, ClamAV periódico, AIDE | Archivos del usuario. |
| `~/.cache`, `~/snap` | ⚪ Excluida | Exclusión al acceso | Entregado en `config/service/clamav/scan-skip.paths`. El análisis periódico sigue recorriendo las rutas excluidas del acceso. |
| Descargas, Escritorio | 🔴 Alta | ClamAV al acceso | `scan-skip add` rechaza estas rutas, su padre, `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root` y el home entero. |
| `/etc` | 🟡 Estándar | AIDE, auditd | Archivos de configuración vigilados por la base de integridad y las reglas de auditoría. |
| `/opt/liltress-workstation` | 🟠 Media | AIDE, diarios locales | Árbol del producto, copias de alerta y diarios de seguridad. |
| `/var/log` | 🟢 Ligera | rotación de diarios, auditd | Los motivos de `chkrootkit` ignoran `/var/log`. |
| Almacenamiento USB | 🟡 Estándar | usbguard | Un almacenamiento nuevo espera una confirmación del escritorio. Teclados, ratones, concentradores y dispositivos ya presentes están permitidos. |
| Archivos empaquetados bajo `/usr` | 🟢 Ligera | debsums, rkhunter | Sumas de control y base de propiedades. Una diferencia de debsums puede reinstalar el paquete. |

## Leyenda

- 🔴 Alta: análisis al acceso más integridad, o una ruta que no se puede excluir.
- 🟠 Media: integridad y el árbol del producto.
- 🟡 Estándar: configuración, auditoría o una confirmación de dispositivo.
- 🟢 Ligera: sumas, propiedades o diarios.
- ⚪ Excluida: nombrada en la lista de exclusión al acceso. No es un hueco silencioso del análisis periódico.

Cambiar un nivel significa cambiar la configuración de la herramienta (`scan-skip`, AIDE, vigilancias de auditd, usbguard), no el color de esta tabla.
