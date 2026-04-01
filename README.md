# ESPHome Alarm System (Clean Baseline)

## Important Notice
- This project was created entirely with OpenAI ChatGPT and Codex.
- The code is an experiment to demonstrate what AI can build.
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
- OLED status display (SSD1306 128x32)
- 4x3 matrix keypad with PIN validation
- RC522 NFC arming/disarming
- Door contacts via MCP23017
- Entry delay, siren trigger, lockout after failed attempts
- Home Assistant buttons for arm/disarm/reset

## Notes
- This setup is a robust baseline, but pin mapping is project-specific.
- On ESP32 Ethernet setups, RMII pins are reserved and cannot be reused freely.
- Strapping pins (for example GPIO0/GPIO2/GPIO15) may affect boot behavior.
- If boot/flash is unstable, review and remap these pins first.
