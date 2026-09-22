# ESPHome Alarm System

## Important Notice
- This project was created with the help of OpenAI ChatGPT and Codex.
- The code is an experiment to demonstrate what AI-assisted development can build.
- No warranty is provided for correctness, safety, or production readiness.
- Use at your own risk.

## Files
- `alarmanlage.yaml`: Main ESPHome configuration
- `secrets.example.yaml`: Example secrets file

## Quick Start
1. Copy `secrets.example.yaml` to `secrets.yaml`.
2. Update `alarm_code` and `nfc_tag_1`.
3. Verify pin mapping against your real wiring.
4. Validate: `esphome config alarmanlage.yaml`
5. Flash: `esphome run alarmanlage.yaml`

## Included Features
- Ethernet (LAN8720) for ESP32-POE-ISO
- OLED status display (SSD1306 128x32), briefly shows the last pressed key in
  plain text before falling back to the masked PIN
- Custom keypad built from 12 individual pushbuttons (no matrix scanning):
  10 digit buttons (0-9) plus dedicated **ARM** and **DISARM** buttons, wired
  to a second MCP23017 I/O expander so the door/siren hub stays untouched
- RC522 NFC arming/disarming
- Door contacts via MCP23017
- 3 status LEDs: Armed, Disarmed, Alarm triggered
- Entry delay, siren trigger, lockout after failed attempts
- Home Assistant buttons for arm/disarm/reset

## Custom Keypad Wiring
Instead of a 4x3 matrix keypad, this build uses 12 separate momentary
pushbuttons, each wired between its own MCP23017 pin and GND (internal
pullups enabled, no diodes needed):

| MCP23017 (`mcp_keypad`, addr 0x21) pin | Function |
|---|---|
| 0-8 | Digits 1-9 |
| 9   | Digit 0 |
| 10  | **ARM** |
| 11  | **DISARM** |

Enter the PIN on the digit buttons, then press **ARM** or **DISARM** to
submit it — a wrong PIN clears the buffer and counts toward the 3-attempt
lockout, same as before. No LED per button is needed: the 3 status LEDs
(Armed / Disarmed / Alarm) already give full feedback, and the OLED flashes
the last pressed key for ~800ms.

## Notes
- This setup is a robust baseline, but pin mapping is project-specific.
- On ESP32 Ethernet setups, RMII pins are reserved and cannot be reused freely.
- Strapping pins (for example GPIO0/GPIO2/GPIO15) may affect boot behavior.
- If boot/flash is unstable, review and remap these pins first.
