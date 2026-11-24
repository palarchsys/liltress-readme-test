<div align="center">

# 🛡️ Instalación 🛡️

</div>

[English](../en/install.md) · [Français](../fr/install.md) · [Deutsch](../de/install.md) · **Español**

## Navigation

- [Índice](README.md)
- **Instalación**
- [Configuración](configure.md)
- [Software y servicios](software.md)
- [Funciones disponibles](functions.md)
- [Correos de alerta](alerts.md)
- [Comandos después de la instalación](commands.md)
- [Mapa de zonas vigiladas](zones.md)
- [VPN](vpn.md)

---

## 📋 Antes de empezar

Tres puntos preceden a la instalación: el sistema, el acceso y las necesidades externas.

## ▪️ Sistema

Ubuntu 26.04 o más reciente, incluida una versión intermedia.<br/>
Se prefiere una instalación limpia.<br/>
Use una consola local o la sesión gráfica ya abierta.

La máquina necesita acceso a Internet.

## ▪️ Acceso

Necesita `sudo` para `./liltress configure`, `./liltress run` y las comprobaciones.

## ▪️ Necesidades externas

| Necesidad | Procedimiento |
| --- | --- |
| Token Ubuntu Pro | Obligatorio. Abra [https://ubuntu.com/pro/dashboard](https://ubuntu.com/pro/dashboard) y copie el token. Solo letras y cifras, longitud de 6 a 100, sin espacio. |
| Contraseña de aplicación | Solo si las alertas por correo están activadas. Use la contraseña de aplicación del proveedor. Nunca la contraseña de la cuenta. Longitud de 8 a 128. Los espacios escritos en la pregunta se retiran. |
| Servidor SMTP | Solo si las alertas están activadas. Forma `host:port`. Ejemplo: `smtp-mail.outlook.com:587`. Los corchetes se rechazan. `swaks` envía un mensaje de prueba. |

## 📋 Curl

```bash
sudo apt-get install -y ca-certificates curl git swaks >/dev/null 2>&1
curl -fsSL https://raw.githubusercontent.com/palarchsys/liltress-workstation/main/script/install.sh | sudo bash
```

`install.sh` pregunta `[O/N]` en el terminal.<br/>
`O` sustituye el árbol instalado por la rama obtenida.<br/>
`O` ejecuta `script/run.sh` cuando `script/configure.sh --check` tiene éxito.

Cualquier otra respuesta se detiene.<br/>
Una comprobación fallida ejecuta `script/configure.sh`.

## 📋 Clone

```bash
sudo git clone https://github.com/palarchsys/liltress-workstation /opt/liltress-workstation
cd /opt/liltress-workstation
sudo ./liltress configure
sudo bash /opt/liltress-workstation/script/version-update.sh
sudo ./liltress verify boot
```

Los comandos posteriores se ejecutan desde `/opt/liltress-workstation`.
