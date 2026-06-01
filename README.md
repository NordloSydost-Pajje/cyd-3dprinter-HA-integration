# CYD Dashboard — cyd-3dprinter-HA-integration

> A feature-rich ESPHome dashboard for the **Cheap Yellow Display (CYD)** that monitors your 3D printer in real time through Home Assistant.

![Banner](docs/images/banner.png)

---

## ✨ Features

- 🎨 **8 themes** — Tactical, Monitor, Orbit, Blueprint, Brutal, Apple, Modern Grid, Skeuomorphic
- 🖌️ **8 color palettes** — independently selectable from themes, synced to Home Assistant
- 👆 **Swipe gestures** — left/right to cycle through themes and palettes
- 📊 **Live printer data** — progress, nozzle & bed temps, layer, time remaining, start/end time
- 🔔 **Toast notifications** — visual feedback on theme/palette changes
- 📱 **Quick panel** — overlay for settings and quick actions
- 💡 **Boot loading screen** — step-by-step startup status (WiFi, HA, UI)
- 💾 **Persistent settings** — theme and palette survive reboots
- 😴 **Auto-sleep** — backlight dims after inactivity
- 🔗 **HA sync** — theme/palette exposed as entities for automation

---

## 📸 Preview

| Orbit | Tactical | Blueprint |
|-------|----------|-----------|
| ![Orbit](docs/images/theme-orbit.png) | ![Tactical](docs/images/theme-tactical.png) | ![Blueprint](docs/images/theme-blueprint.png) |

| Monitor | Brutal | Apple |
|---------|--------|-------|
| ![Monitor](docs/images/theme-monitor.png) | ![Brutal](docs/images/theme-brutal.png) | ![Apple](docs/images/theme-apple.png) |

| Modern Grid | Skeuomorphic |
|-------------|--------------|
| ![Grid](docs/images/theme-grid.png) | ![Skeuo](docs/images/theme-skeuo.png) |

> Note that some of the themes are not finished

---

## ⚡ Quick Start — no clone required

You can use this dashboard **without cloning the repository**. ESPHome will download the dashboard automatically from GitHub.

### 1 — Create your device config

Go into you ESPHome Builder, create a blank config, copy the exemple bellow and fill in your printer entities:

```yaml
# ─────────────────────────────────────────────────────────────────────────────
# CYD 3D Printer Dashboard — User configuration file
# Copy this file, rename it, and fill in your own values.
#
# No clone needed — the full dashboard is loaded from GitHub automatically.
# Source: https://github.com/maelremrem/cyd-3dprinter-HA-integration
# ─────────────────────────────────────────────────────────────────────────────

substitutions:
  device_name: cyd-3d-dashboard       # ESPHome device name (no spaces)
  friendly_name: CYD 3D Dashboard     # Displayed in Home Assistant
  short_name: X1C                     # Short label shown on the display

  # ── API / OTA / Wi-Fi credentials ─────────────────────────────────────────
  # Leave as !secret and define these in your secrets.yaml or put them here
  ha_api_key: !secret ha_api_key
  #or
  #ha_api_key: "" #<-- you can put them here
  ota_password: !secret ota_password
  #or
  #ota_password: ""
  wifi_ssid: !secret wifi_ssid
  wifi_password: !secret wifi_password

  # ── Home Assistant printer entities ────────────────────────────────────────
  # Replace each value with your own entity IDs from the Bambu Lab integration
  # (or any other printer integration exposing similar sensors)
  printer_progress_entity: sensor.YOUR_PRINTER_print_progress
  printer_nozzle_entity: sensor.YOUR_PRINTER_nozzle_temperature
  printer_bed_entity: sensor.YOUR_PRINTER_bed_temperature
  printer_time_entity: sensor.YOUR_PRINTER_remaining_time
  printer_layer_entity: sensor.YOUR_PRINTER_current_layer
  printer_total_layer_entity: sensor.YOUR_PRINTER_total_layer_count
  printer_state_entity: sensor.YOUR_PRINTER_print_status
  printer_start_time_entity: sensor.YOUR_PRINTER_start_time
  printer_end_time_entity: sensor.YOUR_PRINTER_end_time
  printer_power_entity: sensor.YOUR_PRINTER_power
  quick_action_title: "Turn on printer"
  quick_action_entity: switch.YOUR_PRINTER_switch

  ha_clock_entity: sensor.time        # Standard HA time sensor (HH:MM)


# ─────────────────────────────────────────────────────────────────────────────
# Remote package — downloads the full dashboard firmware from GitHub.
# ESPHome caches it locally and refreshes once per day.
# ─────────────────────────────────────────────────────────────────────────────
packages:
  pins: github://maelremrem/cyd-3dprinter-HA-integration/packages/cyd-dashboard-pins.yaml@main
  colors: github://maelremrem/cyd-3dprinter-HA-integration/packages/cyd-dashboard-colors.yaml@main
  dashboard: github://maelremrem/cyd-3dprinter-HA-integration/packages/cyd-dashboard.yaml@main
```

### 2 — Flash

```bash
esphome run my-x1c.yaml
```
Or directly in HomeAssistant via ESPHome Builder

I recommand that you use the USB cable for flashing.

That's it. No local files beyond your config and secrets needed. ESPHome fetches the dashboard code and all defaults from GitHub, caches it locally, and refreshes once per day.

---

### 3 - Usage

There is 3 controls on the touch screen :
- **Swipe right-to-left** → next theme  
- **Swipe left-to-right** → next palette
- **Swipe down-to-up** → quick menu

You will need to allow in HomeAssistant the device to perform action.
Check the guide [here](https://www.home-assistant.io/integrations/esphome#allow-the-device-to-perform-home-assistant-actions)

## 🛒 Hardware

| Component | Details |
|-----------|---------|
| **ESP32 board** | CYD (Cheap Yellow Display) — ESP32 + ILI9341 + XPT2046 |
| **Display** | ILI9341 320×240 px TFT, SPI |
| **Touch** | XPT2046 resistive touchscreen |
| **RGB LED** | Built-in on CYD |

> Any CYD variant with the standard pinout will work. The default pins are configured for the most common version.

---

## 🧰 Prerequisites

- [ESPHome](https://esphome.io) ≥ 2025.2.0 (CLI or Home Assistant add-on)
- [Home Assistant](https://www.home-assistant.io) with the **Bambu Lab integration** installed
- A **Bambu Lab X1C** (or adapt the entity IDs for other models — see [Configuration](#configuration)) I have used [this integration](https://github.com/greghesp/ha-bambulab)

---

## 🎨 Themes & Palettes

Themes and palettes are **independent** — you can mix any theme with any palette.

| # | Theme / Palette |
|---|----------------|
| 1 | Tactical |
| 2 | Monitor |
| 3 | Orbit |
| 4 | Blueprint |
| 5 | Brutal |
| 6 | Apple |
| 7 | Modern Grid |
| 8 | Skeuomorphic |

Both are exposed as `select` entities in Home Assistant (`CYD X1C Dashboard Theme` / `Palette`) and persist across reboots.

You can create you own colors with the ESPHome substitutions (don't forget to comment-out the package)

---

## 🤝 Home Assistant Integration

After flashing, the device will appear automatically in Home Assistant (ESPHome integration). Exposed entities include:

| Entity | Type | Description |
|--------|------|-------------|
| `select.cyd_x1c_dashboard_theme` | Select | Current UI theme |
| `select.cyd_x1c_dashboard_palette` | Select | Current color palette |
| `sensor.cyd_x1c_dashboard_wifi_signal` | Sensor | WiFi RSSI |
| `switch.cyd_x1c_dashboard_backlight` | Switch | Display on/off |

---

## 3D Printing

I have used [this model](https://makerworld.com/en/models/2787810-cyd-desk-buddy-for-bambu-lab-home-assistant)

---

## 🔧 Troubleshooting

| Symptom | Fix |
|---------|-----|
| Touch is off / inverted | Adjust `touch_cal_*` and `touch_swap_xy` / `touch_mirror_*` substitutions |
| Display is mirrored | Set `display_mirror_x` or `display_mirror_y` to `"true"` |
| No printer data | Check entity IDs in `cyd-x1c-substitutions.yaml` match your HA integration |
| Theme not restored after reboot | ESPHome saves theme index to NVS — first reboot after flash resets to default |
| WiFi fallback AP active | Connect to `CYD-X1C-Fallback` with `wifi_ap_password` from `secrets.yaml` |
