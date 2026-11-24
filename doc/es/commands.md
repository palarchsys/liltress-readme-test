<div align="center">

# 🛡️ Comandos después de la instalación 🛡️

</div>

[English](../en/commands.md) · [Français](../fr/commands.md) · [Deutsch](../de/commands.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- [Configuración](configure.md)
- [Software y servicios](software.md)
- [Funciones disponibles](functions.md)
- [Correos de alerta](alerts.md)
- **Comandos después de la instalación**
- [Mapa de zonas vigiladas](zones.md)
- [VPN](vpn.md)

---

Ejecute estos comandos desde `/opt/liltress-workstation`.

## 📋 Comprobar la configuración

```bash
sudo ./liltress check
```

Comprueba `global.conf` y `secrets.conf`.<br/>
La línea de éxito es `Configuration conforme`.

## 📋 Volver a ejecutar el instalador

```bash
sudo ./liltress run
```

Arranca desde los archivos preparados.<br/>
El instalador se niega a empezar si la comprobación no pasa.

## 📋 Probar el camino de las alertas

```bash
sudo ./liltress verify alerts
```

Comprueba el temporizador de la cadena de arranque, el temporizador periódico, ClamAV al acceso y el camino del correo.<br/>
Sin `--no-send`, se envía una sonda cuando las alertas están activadas.<br/>
El asunto termina con `Contrôle`.

La línea de éxito es `Résultat : OK`.

```bash
sudo ./liltress verify alerts --no-send
```

La misma comprobación. Ninguna sonda.

## 📋 Comprobar el puesto

```bash
sudo ./liltress verify workstation
```

Líneas finales: `Check Success : N`, `Check Warnings : N`, `Check Fails : N`.<br/>
El mismo paso comprueba el reenvío SSH, la regla de cortafuegos SSH, USBGuard, el tiempo de espera de sudo y `w-reboot-notice.timer`.

## 📋 Comprobar este arranque

```bash
sudo ./liltress verify boot
```

Éxito: `Contrôle de démarrage : OK`.<br/>
Aún en curso: `Contrôle de démarrage : en cours`, salida `1`.<br/>
Una diferencia: `Contrôle de démarrage : écarts`, salida `1`.

## 📋 Lynis

```bash
sudo ./liltress lynis
```

Ejecuta `lynis audit system --quick --no-colors`.<br/>
El informe está bajo `<source>/log/lynis-<stamp>/`.<br/>
`LYNIS_MIN_SCORE` es 80.

## 📋 Reconstruir las bases

```bash
sudo ./liltress maintain aide
sudo ./liltress maintain rkhunter
```

Éxito de AIDE: `Base de référence AIDE enregistrée.`<br/>
Éxito de rkhunter: `Base de propriétés rkhunter enregistrée.`

## 📋 Sello de espejos de rkhunter

```bash
sudo ./liltress maintain mirrors save
sudo ./liltress maintain mirrors check
```

`mirrors save` escribe el SHA-256 en `/opt/liltress-workstation/cron/rkhunter-mirrors.sha256`, modo `600`.<br/>
`mirrors check` no imprime nada. La salida `0` significa que las huellas coinciden, o que todavía no hay ninguna guardada.

## 📋 Rutas excluidas del acceso

```bash
sudo ./liltress maintain scan-skip list
sudo ./liltress maintain scan-skip add $HOME/.cache/exemple
sudo ./liltress maintain scan-skip del $HOME/.cache/exemple
```

| Acción | Línea impresa |
| --- | --- |
| `list` | una ruta aceptada por línea |
| `add` | `Chemin ajouté : <path>` |
| ya presente | `Ce chemin est déjà exclu : <path>` |
| `del` | `Chemin retiré : <path>` |
| rechazada | `Chemin refusé : <path>` |

La lista completa, incluido el VPN y la restauración de cuarentena, está en [functions.md](functions.md).
