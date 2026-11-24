# Funciones disponibles

[English](../en/functions.md) · [Français](../fr/functions.md) · [Deutsch](../de/functions.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- [Configuración](configure.md)
- [Software y servicios](software.md)
- **Funciones disponibles**
- [Correos de alerta](alerts.md)
- [Comandos después de la instalación](commands.md)
- [Mapa de zonas vigiladas](zones.md)
- [VPN](vpn.md)

Cada comando de abajo es una entrada real de `./liltress`. Ejecútelo desde `/opt/liltress-workstation` salvo que la fila diga otra cosa.

El texto de éxito es la línea que imprime el script. No se traduce.

## liltress

| Comando | Efecto | Éxito o salida |
| --- | --- | --- |
| `./liltress help` | Lista los comandos reconocidos: `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`. `-h` y `--help` muestran la misma ayuda. | Un comando desconocido imprime la línea de comando desconocido y sale con `2`. |
| `sudo ./liltress configure` | Hace las preguntas, escribe `global.conf` en modo `644` y `secrets.conf` en modo `600`, y después arranca `script/run.sh`. | Prueba de correo: `Essai d'envoi accepté.` Correo apagado: `Alertes e-mail coupées. Les contrôles partiront sans envoi.` |
| `sudo ./liltress check` | Comprueba `global.conf` y `secrets.conf` sin reescribirlos. Igual que `script/configure.sh --check`. | `Configuration conforme` |
| `sudo ./liltress run` | Arranca la instalación desde los archivos preparados. Se niega a empezar si la comprobación de configuración falla. Purga, instala paquetes, endurece, ejecuta las comprobaciones de seguridad, construye las bases de AIDE y rkhunter, y después `verify workstation`. | El instalador se detiene si la comprobación falla. |
| `sudo ./liltress lynis` | Ejecuta `lynis audit system --quick --no-colors`. Escribe el informe bajo `<source>/log/lynis-<date>/`. | La última línea empieza por `Rapport :` |
| `sudo ./liltress logs` | Copia una instantánea de diarios a `<source>/log/snapshot-<date>-<host>`. Los archivos privados (`secrets.conf`, claves, contraseñas) no se copian. Un archivo de más de 8 MiB se trunca y el corte se anota en `snapshot/truncated.txt`. | El directorio de la instantánea es el resultado. |

## vpn

| Comando | Efecto |
| --- | --- |
| `sudo ./liltress vpn status` | Muestra el modo `on` u `off`, si el killswitch está armado y el perfil activo. |
| `sudo ./liltress vpn list` | Lista los perfiles registrados: nombre, protocolo, validez, fecha de validación, fecha observada, último intento. |
| `sudo ./liltress vpn import wireguard <brand> <name> <directory or file>` | Copia el perfil bajo `/opt/liltress-workstation/vpn/wireguard/<name>/` y lo prueba. El primer perfil válido arma el modo. |
| `sudo ./liltress vpn import openvpn <brand> <name> <directory or file>` | Copia los archivos citados. Un `auth-user-pass` sin ruta recibe `auth.txt`. El túnel no se abre si el archivo de credenciales tiene menos de dos líneas. |
| `sudo ./liltress vpn test <name>` | Vuelve a probar un perfil. Un fallo no borra la fecha de validación. |
| `sudo ./liltress vpn switch <name>` | Pasa a otro perfil que sigue siendo válido. No baja un perfil. Reinicia los servicios que dependen de él, incluido Unbound. |
| `sudo ./liltress vpn mode on` | Arma el modo VPN. El túnel y el killswitch permanecen hasta una parada manual. SSH no entra en el túnel. |
| `sudo ./liltress vpn mode off` | Detiene el modo VPN. La red pasa a ser directa y el killswitch se retira. Una prueba posterior con éxito no vuelve a armar el modo. |
| `sudo ./liltress vpn rm <name>` | Retira un perfil. El perfil activo no se puede retirar. Borra el directorio y la línea del registro. |
| `sudo ./liltress vpn bypass` | Imprime los prefijos que quedan fuera del túnel. El archivo es `/opt/liltress-workstation/vpn/bypass.cidrs`. |
| `sudo ./liltress vpn bypass reload` | Recarga esos prefijos. Las rutas de desvío se aplican de nuevo cuando el killswitch está armado. |
| `sudo ./liltress vpn refresh-endpoint <name>` | Resuelve de nuevo el nombre DNS del perfil y sustituye el extremo almacenado. |
| `sudo ./liltress vpn default` | Muestra el perfil de arranque, o ninguno. |
| `sudo ./liltress vpn default <name>` | Elige un perfil válido como VPN de arranque. La sesión en curso permanece. |
| `sudo ./liltress vpn default off` | Borra esa elección. Un túnel abierto permanece arriba. |

El detalle de las reglas de importación, las marcas y el killswitch está en [vpn.md](vpn.md).

## verify

| Comando | Efecto | Éxito o salida |
| --- | --- | --- |
| `sudo ./liltress verify alerts` | Comprueba el temporizador de arranque, el temporizador periódico, ClamAV al acceso y el camino del correo. Envía una sonda cuando las alertas están activadas. El asunto termina con `Contrôle`. | `Résultat : OK` |
| `sudo ./liltress verify alerts --no-send` | La misma comprobación. No se envía sonda. | `Résultat : OK` |
| `sudo ./liltress verify workstation` | Comprobación posterior a la instalación. También comprueba el reenvío SSH, la regla de cortafuegos SSH, USBGuard, el tiempo de espera de sudo y `w-reboot-notice.timer`. | `Check Success : N`, `Check Warnings : N`, `Check Fails : N` |
| `sudo ./liltress verify boot` | Ejecuta `script/boot-check.sh` desde las fuentes. | `Contrôle de démarrage : OK` |
| `sudo ./liltress verify boot-check` | La misma comprobación de arranque desde cualquier directorio de la máquina de destino. | En curso: `Contrôle de démarrage : en cours`, salida `1`. Diferencia: `Contrôle de démarrage : écarts`, salida `1`. |
| `sudo ./liltress verify chkrootkit` | Lee un paso de chkrootkit para una persona. Diario: `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`. | No escribe la lista de omisión. |

## maintain

| Comando | Efecto | Línea de éxito |
| --- | --- | --- |
| `sudo ./liltress maintain aide` | Ejecuta `aide --init` y sustituye `/var/lib/aide/aide.db`. Puede tardar varios minutos. | `Base de référence AIDE enregistrée.` |
| `sudo ./liltress maintain rkhunter` | Ejecuta `rkhunter --propupd --nocolors`. | `Base de propriétés rkhunter enregistrée.` |
| `sudo ./liltress maintain chkrootkit <journal>` | Registra líneas nuevas de un `chkrootkit-DATE.log` que ya está bajo `cron/security_logs/`. | `Aucune nouvelle détection à enregistrer.` o `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore` |
| `sudo ./liltress maintain debsums <journal>` | Registra líneas de un `debsums-DATE.log` en `/opt/liltress-workstation/cron/debsums.local.ignore`. El correo de debsums no ofrece este comando: la comprobación ya reinstala cada paquete que sigue denunciado. | Las mismas dos formas, con `debsums.local.ignore`. |
| `sudo ./liltress maintain scan-skip list` | Lista las rutas excluidas del acceso de ClamAV. Archivo: `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`. | Una ruta aceptada por línea. Ruta rechazada en stderr: `Chemin refusé : <path>`. |
| `sudo ./liltress maintain scan-skip add <path>` | Añade una exclusión, reescribe el bloque de exclusión de `/etc/clamav/clamd.conf`, reinicia `clamav-daemon` y después `clamav-clamonacc.service` o `clamonacc.service`. | `Chemin ajouté : <path>` |
| `sudo ./liltress maintain scan-skip del <path>` | Retira una exclusión y reescribe `clamd.conf` del mismo modo. | `Chemin retiré : <path>` |
| `sudo ./liltress maintain mirrors save` | Escribe el SHA-256 de `/var/lib/rkhunter/db/mirrors.dat` en `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, modo `600`. | Sin letrero fijo. El archivo es el resultado. |
| `sudo ./liltress maintain mirrors check` | Compara la huella actual con la guardada. No imprime nada. | Salida `0` si son iguales, o si todavía no hay huella guardada. Cualquier otra salida significa una diferencia. |
| `sudo ./liltress maintain restore <id>` | Restaura un archivo de cuarentena. El id es una línea de `cron/quarantine.registry`. Script: `core/quarantine/restore.sh`. | `fichier remis en place` |
| `sudo ./liltress maintain mail` | Envía una prueba de disposición al destinatario configurado. Script: `core/mail/mail-test.sh`. | El mensaje de prueba es el resultado. |

### Límites de scan-skip

Una ruta se acepta bajo el home de la cuenta del puesto, y por debajo de ese home.

Se rechazan `/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, el home entero, Descargas, Escritorio y su padre.

Ya presente: `Ce chemin est déjà exclu : <path>`.

Ausente al borrar: `Ce chemin n'est pas dans la liste : <path>`, salida `1`.

Una ruta rechazada deja el archivo sin cambios.
