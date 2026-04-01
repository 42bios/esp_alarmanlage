# ESPHome Alarmanlage (bereinigte Basis)

## Wichtiger Hinweis
- Dieses Projekt wurde komplett mit OpenAI ChatGPT bzw. Codex erstellt.
- Der Code ist ein Test, um zu zeigen, was mit KI moeglich ist.
- Keine Gewaehr auf Korrektheit, Sicherheit oder Eignung fuer den produktiven Betrieb.
- Verwendung auf eigene Gefahr.

Dateien:
- `alarmanlage.yaml`: Hauptkonfiguration
- `secrets.example.yaml`: Beispiel fuer Secrets

## Schnellstart
1. `secrets.example.yaml` nach `secrets.yaml` kopieren.
2. `alarm_code` und `nfc_tag_1` anpassen.
3. Pin-Mapping mit deiner Verdrahtung abgleichen.
4. Validieren: `esphome config alarmanlage.yaml`
5. Flashen: `esphome run alarmanlage.yaml`

## Enthaltene Funktionen
- Ethernet (LAN8720) fuer ESP32-POE-ISO
- OLED-Statusanzeige (SSD1306 128x32)
- Matrix-Keypad (4x3) mit PIN-Pruefung
- NFC (RC522) zum Arm/Disarm
- Tuerkontakte ueber MCP23017
- Entry Delay, Sirenen-Trigger, Sperre nach Fehlversuchen
- Home-Assistant Buttons fuer Arm/Disarm/Reset

## Wichtige Hinweise
- Dieses Setup ist als robuste Basis gedacht, Pin-Belegung ist **projektspezifisch**.
- Bei Ethernet auf ESP32 sind RMII-Pins reserviert. Diese nicht fuer andere Komponenten nutzen.
- Strapping-Pins (z. B. GPIO0/GPIO2/GPIO15) koennen Boot-Verhalten beeinflussen.
- Wenn Flash/Boot instabil ist, zuerst diese Pins in der Konfiguration umlegen.
