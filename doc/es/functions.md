<div align="center">

# 🛡️ Funciones disponibles 🛡️

</div>

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

---

## 📋 El lanzador

Estos comandos preparan, instalan o inspeccionan el puesto.

## ▪️ help

Lista los comandos de liltress.

```bash
./liltress help
```

Comandos reconocidos: `configure`, `check`, `run`, `lynis`, `logs`, `vpn`, `verify`, `maintain`, `timer`, `install`, `help`.<br/>
`-h` y `--help` muestran la misma ayuda.<br/>
Un comando desconocido imprime la línea de comando desconocido y sale con `2`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/share/core/run/launch.sh`

## ▪️ configure

Prepara la configuración y luego instala.

```bash
sudo ./liltress configure
```

El script hace las preguntas, escribe `global.conf` modo `644` y `secrets.conf` modo `600`, y luego arranca `script/run.sh`.<br/>
El token Ubuntu Pro es obligatorio. Las alertas se responden con `Y` o `n`.<br/>
Si las alertas están activas, se piden el servidor SMTP, el login, la contraseña de aplicación y la dirección From, y `swaks` debe recibir un `250`.

`MODE_TEST` se responde con `0` o `1`. El valor entregado es `1`. Una opción desconocida se rechaza.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Essai d'envoi accepté.`

Éxito : `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

Fuente : `src/workstation/script/configure.sh`

## ▪️ check

Comprueba `global.conf` y `secrets.conf` sin reescribirlos.

```bash
sudo ./liltress check
```

El código `0` significa que ambos archivos son conformes.<br/>
`secrets.conf` está en modo `600`, así que hace falta `sudo` para leerlo.<br/>
Es la misma comprobación que `script/configure.sh --check`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Configuration conforme`

Fuente : `src/workstation/script/configure.sh`

## ▪️ run

Arranca la instalación desde los archivos ya preparados.

```bash
sudo ./liltress run
```

El instalador no arranca si la comprobación de la configuración falla.<br/>
Después purga, instala paquetes, endurece, ejecuta los controles, construye las bases de AIDE y rkhunter, y luego `verify/workstation.sh`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/script/run.sh`

## ▪️ lynis

Ejecuta una auditoría Lynis del puesto.

```bash
sudo ./liltress lynis
```

El comando real es `lynis audit system --quick --no-colors`.<br/>
El informe se escribe bajo `<source>/log/lynis-<date>/`.<br/>
La última línea muestra `Rapport :` seguido de ese directorio.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Rapport :`

Fuente : `src/share/core/run/lynis.sh`

## ▪️ logs

Copia una instantánea de diarios en las fuentes.

```bash
sudo ./liltress logs
```

El directorio creado es `<source>/log/snapshot-<date>-<host>`.<br/>
Los archivos privados (`secrets.conf`, claves, contraseñas) no se copian.<br/>
Un archivo de más de 8 MiB se trunca. El corte se anota en `snapshot/truncated.txt`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/share/script/log-collect.sh`

## 📋 VPN

Estos comandos gestionan los perfiles.

## ▪️ vpn status

Muestra el modo VPN, el killswitch y el perfil activo.

```bash
sudo ./liltress vpn status
```

El modo es `on` u `off`.<br/>
El killswitch está armado o desarmado.<br/>
Si no hay perfil activo, la línea lo dice.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn list

Lista los perfiles VPN registrados.

```bash
sudo ./liltress vpn list
```

Cada línea da el nombre, el protocolo, la validez, la fecha de validación, la fecha observada y la fecha del último intento.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import wireguard <marque> <nom> <dossier ou fichier>

Importa un perfil WireGuard.

```bash
sudo ./liltress vpn import wireguard <marque> <nom> <dossier ou fichier>
```

El archivo se copia bajo `/opt/liltress-workstation/vpn/wireguard/<nom>/`.<br/>
Una prueba valida el perfil.<br/>
El modo se arma con el primer perfil válido.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn import openvpn <marque> <nom> <dossier ou fichier>

Importa un perfil OpenVPN.

```bash
sudo ./liltress vpn import openvpn <marque> <nom> <dossier ou fichier>
```

Se copian los archivos citados por el perfil.<br/>
Un `auth-user-pass` sin ruta recibe `auth.txt`.<br/>
El túnel no se abre si el archivo de credenciales tiene menos de dos líneas.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn test <nom>

Vuelve a probar un perfil y lo valida de nuevo si responde.

```bash
sudo ./liltress vpn test <nom>
```

Se guardan tres fechas: validación, último estado observado, último intento.<br/>
Un fallo no quita la fecha de validación.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn switch <nom>

Cambia a otro perfil que ya es válido.

```bash
sudo ./liltress vpn switch <nom>
```

El comando no baja un perfil.<br/>
Solo elige un perfil que sigue siendo válido.<br/>
Los servicios que dependen de él, incluido Unbound, se reinician.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode on

Arma el modo VPN.

```bash
sudo ./liltress vpn mode on
```

El túnel y el killswitch siguen hasta una parada manual.<br/>
SSH no entra en el túnel.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn mode off

Detiene el modo VPN.

```bash
sudo ./liltress vpn mode off
```

La red vuelve a ser directa y el killswitch se retira.<br/>
Un intento posterior que tenga éxito no vuelve a armar el modo.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn rm <nom>

Retira un perfil VPN.

```bash
sudo ./liltress vpn rm <nom>
```

El perfil activo no se puede retirar.<br/>
El comando borra el directorio y la línea del registro.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass

Muestra los prefijos que quedan fuera del túnel.

```bash
sudo ./liltress vpn bypass
```

La lista leída es `/opt/liltress-workstation/vpn/bypass.cidrs`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn bypass reload

Recarga los prefijos que quedan fuera del túnel.

```bash
sudo ./liltress vpn bypass reload
```

Las rutas de desvío se aplican de nuevo si el killswitch está armado.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

## ▪️ vpn refresh-endpoint <nom>

Vuelve a resolver la dirección del servidor VPN.

```bash
sudo ./liltress vpn refresh-endpoint <nom>
```

El nombre DNS del perfil se resuelve de nuevo.<br/>
La dirección sustituye el extremo guardado.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/service/vpn/cli.sh`

Las marcas, el killswitch y el perfil de arranque están en [vpn.md](vpn.md).

## 📋 Controles

Estos comandos comprueban. No reconstruyen una base.

## ▪️ verify alerts

Comprueba la cadena de alertas y envía una sonda.

```bash
sudo ./liltress verify alerts
```

La pasada controla el temporizador de arranque, el temporizador periódico, ClamAV al acceso y la vía del correo.<br/>
Se envía un mensaje de prueba si las alertas están activas. El asunto termina en `Contrôle`.<br/>
`-h` y `--help` muestran el uso. Cualquier otra opción se rechaza.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Résultat : OK`

Fuente : `src/workstation/script/verify/alerts.sh`

## ▪️ verify alerts --no-send

La misma comprobación de alertas, sin sonda.

```bash
sudo ./liltress verify alerts --no-send
```

El mismo alcance que `verify/alerts.sh`.<br/>
No se envía ningún mensaje de prueba.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Résultat : OK`

Fuente : `src/workstation/script/verify/alerts.sh`

## ▪️ verify workstation

Comprueba el puesto después de la instalación.

```bash
sudo ./liltress verify workstation
```

El primer argumento es el directorio de las fuentes.<br/>
Las líneas finales son `Check Success : N`, `Check Warnings : N` y `Check Fails : N`.<br/>
La pasada también controla el reenvío SSH, la regla de cortafuegos SSH, USBGuard, el tiempo de sudo y `w-reboot-notice.timer`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Check Success : N`

Fuente : `src/workstation/script/verify/workstation.sh`

## ▪️ verify boot-check

Comprueba los servicios de seguridad de este arranque.

```bash
sudo ./liltress verify boot-check
```

Una pasada aún en curso muestra `Contrôle de démarrage : en cours` y sale con `1`.<br/>
Una diferencia muestra `Contrôle de démarrage : écarts` y sale con `1`.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Contrôle de démarrage : OK`

Fuente : `src/workstation/script/boot-check.sh`

## ▪️ verify boot

Lanza `boot-check.sh` desde las fuentes.

```bash
sudo ./liltress verify boot
```

`script/verify/boot.sh` ejecuta `script/boot-check.sh` del mismo árbol.<br/>
El resultado es el de `boot-check.sh`.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Éxito : `Contrôle de démarrage : OK`

Fuente : `src/workstation/script/verify/boot.sh`

## ▪️ verify chkrootkit

Lee una pasada chkrootkit para una persona.

```bash
sudo ./liltress verify chkrootkit
```

El diario es `/var/log/liltress-workstation/chkrootkit-fp-<date>.log`.<br/>
Este comando no escribe nada en la lista de ignorados.

Láncelo desde `/opt/liltress-workstation`, o desde el clon.

Fuente : `src/workstation/script/verify/chkrootkit-fp.sh`

## 📋 Mantenimiento

Estos comandos reconstruyen una base, registran una lista de ignorados, o devuelven un archivo.

## ▪️ maintain aide

Reconstruye la base AIDE.

```bash
sudo ./liltress maintain aide
```

El comando ejecuta `aide --init` y sustituye `/var/lib/aide/aide.db`.<br/>
El cálculo puede tardar varios minutos.<br/>
Es el comando que adjunta el correo de AIDE.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Base de référence AIDE enregistrée.`

Fuente : `src/workstation/service/aide/refresh-db.sh`

Instalado : `/opt/liltress-workstation/service/aide/refresh-db.sh`

## ▪️ maintain rkhunter

Reconstruye la base de propiedades de rkhunter.

```bash
sudo ./liltress maintain rkhunter
```

El comando ejecuta `rkhunter --propupd --nocolors`.<br/>
Es el comando del correo de aviso de rkhunter.<br/>
El correo `mirrors.dat modifié hors rkhunter --update` no lo adjunta.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Base de propriétés rkhunter enregistrée.`

Fuente : `src/share/service/rkhunter/refresh-db.sh`

Instalado : `/opt/liltress-workstation/service/rkhunter/refresh-db.sh`

## ▪️ maintain chkrootkit <journal>

Registra líneas de un diario chkrootkit como ignoradas.

```bash
sudo ./liltress maintain chkrootkit <journal>
```

El archivo debe ser un `chkrootkit-DATE.log` ya bajo `cron/security_logs/`.<br/>
Las líneas nuevas se añaden a `/opt/liltress-workstation/cron/chkrootkit.local.ignore`.<br/>
Si no hay nada nuevo, la línea es `Aucune nouvelle détection à enregistrer.` Si no, nombra el número registrado en ese archivo.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Aucune nouvelle détection à enregistrer.`

Éxito : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/chkrootkit.local.ignore`

Fuente : `src/workstation/service/chkrootkit/ignore-log.sh`

Instalado : `/opt/liltress-workstation/service/chkrootkit/ignore-log.sh`

## ▪️ maintain debsums <journal>

Registra líneas de un diario debsums como ignoradas.

```bash
sudo ./liltress maintain debsums <journal>
```

El archivo debe ser un `debsums-DATE.log` bajo `cron/security_logs/`.<br/>
Los motivos van a `/opt/liltress-workstation/cron/debsums.local.ignore`.<br/>
Las líneas de éxito son las mismas que para chkrootkit. El correo debsums no ofrece este comando: la comprobación ya reinstala cada paquete que sigue señalado.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Aucune nouvelle détection à enregistrer.`

Éxito : `N détection(s) enregistrée(s) dans /opt/liltress-workstation/cron/debsums.local.ignore`

Fuente : `src/workstation/service/debsums/ignore-log.sh`

Instalado : `/opt/liltress-workstation/service/debsums/ignore-log.sh`

## ▪️ maintain scan-skip list

Lista las rutas excluidas del acceso de ClamAV.

```bash
sudo ./liltress maintain scan-skip list
```

Una ruta aceptada por línea.<br/>
Una ruta rechazada se escribe en stderr: `Chemin refusé : <path>`.<br/>
El archivo es `/opt/liltress-workstation/config/service/clamav/scan-skip.paths`.

Puede lanzarse desde cualquier directorio de la máquina.

Fuente : `src/workstation/script/scan-skip.sh`

Instalado : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip add <chemin>

Añade una ruta excluida del acceso de ClamAV.

```bash
sudo ./liltress maintain scan-skip add <chemin>
```

Ya presente: `Ce chemin est déjà exclu : <path>`.<br/>
Un rechazo deja el archivo igual y muestra `Chemin refusé : <path>`.<br/>
Una ruta se acepta bajo el home de la cuenta del puesto, y por debajo de ese home.

`/`, `/home`, `/opt`, `/tmp`, `/etc`, `/usr`, `/var`, `/boot`, `/root`, el home entero, Descargas, Escritorio y su padre se rechazan.<br/>
Tras un cambio, el comando reescribe el bloque de exclusión de `/etc/clamav/clamd.conf`, reinicia `clamav-daemon`, y luego `clamav-clamonacc.service` o `clamonacc.service`.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Chemin ajouté : <path>`

Fuente : `src/workstation/script/scan-skip.sh`

Instalado : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain scan-skip del <chemin>

Quita una ruta de la lista de exclusión de ClamAV.

```bash
sudo ./liltress maintain scan-skip del <chemin>
```

Si no está, el comando muestra `Ce chemin n'est pas dans la liste : <path>` y sale con `1`.<br/>
La misma reescritura de `clamd.conf` sigue a una retirada lograda.

Puede lanzarse desde cualquier directorio de la máquina.

Éxito : `Chemin retiré : <path>`

Fuente : `src/workstation/script/scan-skip.sh`

Instalado : `/opt/liltress-workstation/script/scan-skip.sh`

## ▪️ maintain mirrors save

Registra la huella de los espejos de rkhunter.

```bash
sudo ./liltress maintain mirrors save
```

El SHA-256 de `/var/lib/rkhunter/db/mirrors.dat` se escribe en `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, modo `600`.<br/>
`refresh-rkhunter-db.sh` lo hace solo después de que `rkhunter --update` devuelva `0` o `2`.

Puede lanzarse desde cualquier directorio de la máquina.

Fuente : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Instalado : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain mirrors check

Compara la huella actual de rkhunter con la guardada.

```bash
sudo ./liltress maintain mirrors check
```

No se imprime nada.<br/>
La salida `0` significa que las dos huellas son iguales, o que aún no hay ninguna guardada.<br/>
Una diferencia sale con un código distinto de `0`.

Puede lanzarse desde cualquier directorio de la máquina.

Fuente : `src/workstation/service/cron/watchdogs/record-mirrors.sh`

Instalado : `/opt/liltress-workstation/service/cron/watchdogs/record-mirrors.sh`

## ▪️ maintain restore <id>

Devuelve un archivo de cuarentena a su lugar de origen.

```bash
sudo ./liltress maintain restore <id>
```

El identificador es una línea de `cron/quarantine.registry`.<br/>
El script es `core/quarantine/restore.sh`.

Puede lanzarse desde cualquier directorio de la máquina.

Fuente : `src/share/core/quarantine/restore.sh`

Instalado : `/opt/liltress-workstation/core/quarantine/restore.sh`

## ▪️ maintain mail

Envía un correo de prueba.

```bash
sudo ./liltress maintain mail
```

El mensaje sirve para ver el ancho y el alto.<br/>
El destinatario sigue siendo el de la configuración instalada.<br/>
El script es `core/mail/mail-test.sh`.

Puede lanzarse desde cualquier directorio de la máquina.

Fuente : `src/share/core/mail/mail-test.sh`

Instalado : `/opt/liltress-workstation/core/mail/mail-test.sh`
