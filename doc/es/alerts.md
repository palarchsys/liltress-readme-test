# Correos de alerta

[English](../en/alerts.md) · [Français](../fr/alerts.md) · [Deutsch](../de/alerts.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- [Configuración](configure.md)
- [Software y servicios](software.md)
- [Funciones disponibles](functions.md)
- **Correos de alerta**
- [Comandos después de la instalación](commands.md)
- [Mapa de zonas vigiladas](zones.md)
- [VPN](vpn.md)

Cada archivo de alerta se guarda bajo `/opt/liltress-workstation/cron/alerts`.

El correo sigue `LANGUAGE`.

El correo solo sale cuando `MAIL_ALERTS=1`, `WATCHDOG_MAIL` está definido y `/opt/liltress-workstation/service/cron/watchdogs/send.sh` es ejecutable.

Una comprobación limpia no envía correo.

Si una línea del diario es desconocida, no ejecute el comando. Conserve el correo.

Una comprobación que se detiene antes de su final normal envía correo. El correo nombra la comprobación y la señal o el código de salida.

## AIDE

El mensaje contiene el resumen, un extracto del diario, un bloque de verificación por hallazgo, después la nota y el comando.

La forma del diario es `/opt/liltress-workstation/cron/security_logs/aide-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain aide
```

El comando reconstruye la base de AIDE desde el disco actual.

La línea de éxito es `Base de référence AIDE enregistrée.`

## chkrootkit

El mensaje contiene el resumen, el extracto, un bloque de verificación por hallazgo, después la nota y el comando.

```bash
sudo ./liltress maintain chkrootkit '/opt/liltress-workstation/cron/security_logs/chkrootkit-YYYYMMDD-HHMMSS.log'
```

El comando añade líneas nuevas a `/opt/liltress-workstation/cron/chkrootkit.local.ignore`.

El éxito es `Aucune nouvelle détection à enregistrer.` o `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`.

Una detección confirmada mueve el archivo y adjunta en su lugar el comando de restauración. El comando de registro permanece solo cuando ningún archivo confirmado fue movido.

## debsums

El mensaje contiene el resumen y el extracto. No añade un bloque de comando.

La comprobación reinstala cada paquete que sigue denunciado con `apt-get install --reinstall`.

El correo nombra cada paquete restaurado.

La línea de éxito es `La somme correspond.`

Un paquete cuya suma sigue siendo distinta, o cuya reinstalación falla, permanece en el correo.

Una línea de `/var/lib/rkhunter/db/mirrors.dat` se retira antes de esa reinstalación solo cuando su SHA-256 actual es igual a `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`.

Una línea cambiada bajo `/usr/lib/*/perl-base/unicore/` del paquete `perl-base` se retira antes de esa reinstalación. El archivo entregado por Ubuntu no coincide con esa suma del paquete.

## ClamAV al acceso

El mensaje contiene la firma, el nombre del archivo, la ruta original, la ruta de cuarentena, la hora, el tipo de detección, las acciones, una recomendación breve y un comando de restauración por archivo movido.

Un archivo cuyo nombre coincide con `.clamav-quarantine-lock.*` no envía correo. El script sale con `0` antes del diario y antes de `log_alert`.

```bash
sudo bash /opt/liltress-workstation/core/quarantine/restore.sh <id>
```

La línea de éxito es `fichier remis en place`.

El registro es `/var/lib/liltress-workstation/watchdog/quarantine.registry`.

## Análisis de arranque y análisis periódico de ClamAV

Ambos mensajes contienen el resumen y el extracto del diario. Un archivo movido añade un comando de restauración, el mismo comando que al acceso.

La forma del diario es `/opt/liltress-workstation/cron/security_logs/clamav-YYYYMMDD-HHMMSS.log`.

La cadena periódica se ejecuta cada hora. El mismo análisis también se ejecuta una vez en la cadena de arranque.

El recorrido periódico incluye las rutas excluidas del análisis al acceso.

Un archivo ilegible, sin línea `FOUND`, no envía correo. Las líneas `Known viruses` e `Infected files` no se copian al correo.

Un `clamscan` detenido por una señal envía correo. El correo nombra la señal y cada archivo no leído. Los demás archivos del paso se analizan. Dos caídas seguidas detienen el paso de ClamAV. El correo dice entonces que los archivos restantes no se leyeron.

## rkhunter

El correo de aviso contiene el resumen, el extracto, un bloque de verificación por hallazgo, después la nota y el comando.

El correo `mirrors.dat modifié hors rkhunter --update` no añade ese bloque. Un correo de error tampoco.

La forma del diario es `/opt/liltress-workstation/cron/security_logs/rkhunter-YYYYMMDD-HHMMSS.log`.

```bash
sudo ./liltress maintain rkhunter
```

La línea de éxito es `Base de propriétés rkhunter enregistrée.`

Una detección confirmada mueve el archivo y adjunta en su lugar el comando de restauración.

Los archivos de `/dev/shm` y el socket `/dev/log` están permitidos en `rkhunter.conf.local`. No producen `Suspicious file types found in /dev`.

## Aviso de reinicio

El mensaje es `Un redémarrage est utile. La machine n'a pas été redémarrée.`

Este correo no añade un bloque de comando.

El mismo conjunto de paquetes no vuelve a enviar el correo.

El script no reinicia la máquina.

La forma del diario es `/opt/liltress-workstation/cron/security_logs/reboot-notice-YYYYMMDD-HHMMSS.log`.
