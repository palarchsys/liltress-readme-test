<div align="center">

# 🛡️ Configuración 🛡️

</div>

[English](../en/configure.md) · [Français](../fr/configure.md) · [Deutsch](../de/configure.md) · **Español**

## Navigation

- [Índice](README.md)
- [Instalación](install.md)
- **Configuración**
- [Software y servicios](software.md)
- [Funciones disponibles](functions.md)
- [Correos de alerta](alerts.md)
- [Comandos después de la instalación](commands.md)
- [Mapa de zonas vigiladas](zones.md)
- [VPN](vpn.md)

---

`sudo ./liltress configure` hace las preguntas de abajo, escribe `global.conf` en modo `644` y `secrets.conf` en modo `600` como root, y después arranca `script/run.sh`.<br/>
La primera pregunta elige el idioma.<br/>
El valor es `LANGUAGE`.

El valor por defecto es el inglés.<br/>
`MODE_TEST` se responde con `0` o `1`. El valor entregado es `1`.<br/>
Una opción desconocida se rechaza.

| Pregunta | Respuesta esperada |
| --- | --- |
| Token Ubuntu Pro | Letras y cifras, de 6 a 100, sin espacio. Obligatorio. |
| Activar el correo | `Y` o `y` activa el correo. `N`, `n` o `non` lo desactiva. Desactivado no instala Postfix. Cualquier otra respuesta muestra `Répondez Y ou N.` |
| Servidor | `host:port`, por ejemplo `smtp-mail.outlook.com:587`. Sin corchetes. Solo si las alertas están activadas. |
| Login | Una dirección de correo. |
| Password | La contraseña de aplicación del proveedor, de 8 a 128 caracteres. Los espacios se retiran. Las comillas, `$`, `\` y las comillas invertidas se rechazan. Nunca la contraseña de la cuenta. |
| From | Una dirección de correo. `WATCHDOG_MAIL` recibe la misma dirección. |

Cuando las alertas están activadas, `swaks` debe recibir un `250`.<br/>
La línea de éxito de esa prueba es `Essai d'envoi accepté.`<br/>
Cuando las alertas siguen apagadas, la línea es `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

## 📋 Comprobación sin reescritura

```bash
sudo ./liltress check
```

El comando comprueba `global.conf` y `secrets.conf` y no los reescribe.<br/>
`secrets.conf` está en modo `600`, así que hace falta `sudo` para leerlo.<br/>
El código de salida `0` significa que ambos archivos son conformes.

La línea de éxito es `Configuration conforme`.<br/>
Es la misma comprobación que `script/configure.sh --check`.
