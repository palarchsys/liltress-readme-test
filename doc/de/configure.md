<div align="center">

# 🛡️ Konfiguration 🛡️

</div>

[English](../en/configure.md) · [Français](../fr/configure.md) · **Deutsch** · [Español](../es/configure.md)

## Navigation

- [Index](README.md)
- [Installation](install.md)
- **Konfiguration**
- [Software und Dienste](software.md)
- [Verfügbare Funktionen](functions.md)
- [Alarm-E-Mails](alerts.md)
- [Befehle nach der Installation](commands.md)
- [Überwachte Bereiche](zones.md)
- [VPN](vpn.md)

---

`sudo ./liltress configure` stellt die Fragen unten, schreibt `global.conf` mit Modus `644` und `secrets.conf` mit Modus `600` als root und startet dann `script/run.sh`.<br/>
Die erste Frage wählt die Sprache.<br/>
Der Wert ist `LANGUAGE`.

Die Vorgabe ist Englisch.<br/>
`MODE_TEST` wird mit `0` oder `1` beantwortet. Der gelieferte Vorgabewert ist `1`.<br/>
Eine unbekannte Option wird abgelehnt.

| Frage | Erwartete Antwort |
| --- | --- |
| Ubuntu-Pro-Token | Buchstaben und Ziffern, 6 bis 100, ohne Leerzeichen. Pflicht. |
| Mail aktivieren | `Y` oder `y` schaltet Mail ein. `N`, `n` oder `non` schaltet sie aus. Aus installiert Postfix nicht. Jede andere Antwort zeigt `Répondez Y ou N.` |
| Server | `host:port`, zum Beispiel `smtp-mail.outlook.com:587`. Keine Klammern. Nur wenn Alarme aktiv sind. |
| Login | Eine E-Mail-Adresse. |
| Password | Das Anwendungspasswort, 8 bis 128 Zeichen. Leerzeichen werden entfernt. Anführungszeichen, `$`, `\` und Backticks werden abgelehnt. Niemals das Kontopasswort. |
| From | Eine E-Mail-Adresse. `WATCHDOG_MAIL` erhält dieselbe Adresse. |

Wenn Alarme aktiv sind, muss `swaks` ein `250` erhalten.<br/>
Die Erfolgszeile dieses Tests ist `Essai d'envoi accepté.`<br/>
Wenn Alarme aus bleiben, lautet die Zeile `Alertes e-mail coupées. Les contrôles partiront sans envoi.`

## 📋 Prüfung ohne Neuschreiben

```bash
sudo ./liltress check
```

Der Befehl prüft `global.conf` und `secrets.conf` und schreibt sie nicht neu.<br/>
`secrets.conf` hat Modus `600`, deshalb ist `sudo` zum Lesen nötig.<br/>
Code `0` bedeutet, dass beide Dateien stimmen.

Die Erfolgszeile ist `Configuration conforme`.<br/>
Das ist dieselbe Prüfung wie `script/configure.sh --check`.
