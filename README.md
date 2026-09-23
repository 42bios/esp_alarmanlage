# ESPHome Alarm-Bedieneinheit

## Important Notice
- This project was created with the help of Anthropic's Claude (Claude Code).
- The code is an experiment to demonstrate what AI-assisted development can build.
- No warranty is provided for correctness, safety, or production readiness.
- Use at your own risk.

## Architecture
This ESP32 device is a **pure input/output panel (HMI)** — it has **no alarm
logic on board**. All decision-making (PIN validation, arm/disarm state,
door contacts, entry delay, siren, lockout, users) lives in **Home
Assistant** as a package/automation (e.g. built on top of the
`alarm_control_panel` domain, or the Alarmo add-on).

The ESP32 only:
- reports raw button presses (digits 0-9, ARM, DISARM) as `binary_sensor`
  entities over the ESPHome native API
- exposes 4 status LEDs (Armed / Disarmed / Alarm / Aktiv) as `light`
  entities that Home Assistant turns on/off

Everything else — collecting digits into a PIN, comparing it to a code,
tracking failed attempts/lockout, arming/disarming, reacting to door
contacts elsewhere in the house, triggering a siren — is a Home Assistant
automation/package that listens to these 12 button entities and drives the
4 LED entities. This keeps the device dumb and swappable, and all "software"
logic (delays, users, PINs) is edited in HA instead of reflashed firmware.

## Files
- `alarmanlage.yaml`: ESPHome configuration for the panel (buttons + LEDs only)
- `secrets.example.yaml`: Example secrets file (currently barely needed —
  no PIN/NFC secrets live on the device anymore)

## Quick Start
1. Copy `secrets.example.yaml` to `secrets.yaml` if you want an API encryption key.
2. Verify pin mapping against your real wiring.
3. Validate: `esphome config alarmanlage.yaml`
4. Flash: `esphome run alarmanlage.yaml`
5. Add the device in Home Assistant and build the arm/disarm/PIN logic there
   (package/automation, not covered by this repo yet).

## Hardware / Enclosure
Designed to fit behind a **JUNG LS 990** single-gang cover frame, mounted
into a standard flush-mount ("Kaiser") box, with a custom laser-cut face
plate replacing the switch insert.

- **1 MCP23017** (I2C, address `0x20`) handles all 16 GPIOs needed:
  12 inputs (buttons) + 4 outputs (LEDs) — no second expander required.
- **12 individual pushbuttons** (digits 1-9, 0, ARM, DISARM), each wired
  directly to its own MCP23017 pin and GND, with the internal pullup
  enabled — no diode matrix, no scanning.
- **4 status LEDs** (Armed / Disarmed / Alarm / Aktiv) mounted in a row
  above the button grid.
- No OLED and no NFC reader on this panel: a single LS990 gang (~50x50mm
  usable) is too small to fit a display or an RC522 antenna alongside 12
  buttons. Status is shown on the Home Assistant dashboard/app instead; NFC
  (if wanted later) should be its own separate panel.
- ⚠️ The ESP32-POE-ISO board itself (~65x51mm) plus its RJ45 jack does
  **not** fit inside a standard round flush-mount box together with the
  button PCB. Plan for a deep/rectangular back box, or mount the ESP32
  module separately (e.g. in a nearby junction box) with only the button
  wiring and LEDs going through the cover plate.
- Laser-cut cover template: see `ls990_panel_lasercut.svg` — dimensions are
  **approximate placeholders**, verify against your actual LS990 frame /
  blank cover before cutting.

## Notes
- This setup is a robust baseline, but pin mapping is project-specific.
- On ESP32 Ethernet setups, RMII pins are reserved and cannot be reused freely.
- Strapping pins (for example GPIO0/GPIO2/GPIO15) may affect boot behavior.
- If boot/flash is unstable, review and remap these pins first.
