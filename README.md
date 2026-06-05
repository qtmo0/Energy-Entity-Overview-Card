# button-card — Generic Toggle Card

A reusable, fully configurable `custom:button-card` for Home Assistant that toggles a switch entity and displays up to four sensor values in the subtitle. Fully configurable via variables — no code changes needed. Supports dynamic background color when a sensor threshold is exceeded, on/off icon states, and an optional native HA confirmation dialog before toggling.

---

## Features

- Toggle any switch entity on tap
- Display up to 4 sensor values with individual units and decimal places
- Background color changes when a threshold entity exceeds a configurable value
- Icon color reflects on/off state
- Optional native HA confirmation dialog before toggling

---

## Preview

![Card in default state](https://raw.githubusercontent.com/qtmo0/Energy-Entity-Overview-Card/main/screenshot_1.png)

![Card with threshold background active](https://raw.githubusercontent.com/qtmo0/Energy-Entity-Overview-Card/main/screenshot_2.png)

---

## Requirements

- [custom:button-card](https://github.com/custom-cards/button-card) installed via HACS

---

## Configuration

All options are set via the `variables` block — no code changes needed.

### Variables Reference

| Variable | Type | Required | Default | Description |
|---|---|---|---|---|
| `entity_toggle` | `entity_id` | ✅ | — | Entity to toggle on tap |
| `entity_value_1` | `entity_id` | ✅ | — | Primary sensor to display |
| `entity_value_2` | `entity_id` \| `null` | ❌ | `null` | Second sensor (omit with `null`) |
| `entity_value_3` | `entity_id` \| `null` | ❌ | `null` | Third sensor (omit with `null`) |
| `entity_value_4` | `entity_id` \| `null` | ❌ | `null` | Fourth sensor (omit with `null`) |
| `threshold_entity` | `entity_id` | ✅ | — | Entity whose value triggers background change |
| `threshold_value` | `number` | ✅ | — | Value above which background changes |
| `card_name` | `string` | ✅ | — | Display name on the card |
| `unit_1` | `string` | ❌ | `""` | Unit for sensor 1 (e.g. `W`, `°C`) |
| `unit_2` | `string` | ❌ | `""` | Unit for sensor 2 |
| `unit_3` | `string` | ❌ | `""` | Unit for sensor 3 |
| `unit_4` | `string` | ❌ | `""` | Unit for sensor 4 |
| `decimals_1` | `number` | ❌ | `1` | Decimal places for sensor 1 |
| `decimals_2` | `number` | ❌ | `1` | Decimal places for sensor 2 |
| `decimals_3` | `number` | ❌ | `1` | Decimal places for sensor 3 |
| `decimals_4` | `number` | ❌ | `1` | Decimal places for sensor 4 |
| `icon` | `mdi:icon` | ✅ | — | Any MDI icon string |
| `color_active` | `hex` | ❌ | `#97BE5A` | Icon color when entity is `on` |
| `color_inactive` | `hex` | ❌ | `#9E9E9E` | Icon color when entity is `off` |
| `color_threshold_bg` | `hex` | ❌ | `#FFC470` | Background color when threshold exceeded |
| `threshold_bg_opacity` | `float` | ❌ | `0.6` | Opacity of threshold background (0.0–1.0) |
| `confirmation_enabled` | `boolean` | ❌ | `false` | Show HA confirmation dialog before toggle |
| `confirmation_text` | `string` | ❌ | `""` | Text shown in the confirmation dialog |

---

## Examples

### Basic — single sensor, no confirmation

```yaml
variables:
  entity_toggle: switch.my_switch
  entity_value_1: sensor.my_power
  entity_value_2: null
  entity_value_3: null
  entity_value_4: null
  threshold_entity: sensor.my_power
  threshold_value: 10
  card_name: My Device
  unit_1: W
  decimals_1: 0
  icon: mdi:television
  color_active: "#97BE5A"
  color_inactive: "#9E9E9E"
  color_threshold_bg: "#FFC470"
  threshold_bg_opacity: 0.6
  confirmation_enabled: false
  confirmation_text: ""
```

### Advanced — 4 sensors, with confirmation

```yaml
variables:
  entity_toggle: switch.tasmota_mk4
  entity_value_1: sensor.tasmota_mk4_energy_power
  entity_value_2: sensor.prusalink_nozzle_temperature
  entity_value_3: sensor.prusalink_heatbed_temperature
  entity_value_4: sensor.ble_temperature_thermometer_enclosure
  threshold_entity: sensor.tasmota_mk4_energy_power
  threshold_value: 5
  card_name: Prusa MK4
  unit_1: W
  unit_2: "°C"
  unit_3: "°C"
  unit_4: "°C"
  decimals_1: 0
  decimals_2: 0
  decimals_3: 0
  decimals_4: 1
  icon: mdi:printer-3d
  color_active: "#97BE5A"
  color_inactive: "#9E9E9E"
  color_threshold_bg: "#FFC470"
  threshold_bg_opacity: 0.6
  confirmation_enabled: true
  confirmation_text: Prusa MK4 wirklich schalten?
```

![Card with confirmation dialog](https://raw.githubusercontent.com/qtmo0/Energy-Entity-Overview-Card/main/screenshot_3.png)

---

## Behavior

### Icon
- Displays the configured `mdi:` icon in a rounded square
- Background and icon color reflect the on/off state of `entity_toggle`

### Subtitle
- Up to 4 sensor values joined by `·`
- Values formatted with configured decimal places and units
- Shows `—` when a sensor is `unavailable` or `unknown`

### Background
- Card background turns `color_threshold_bg` (at `threshold_bg_opacity`) when `threshold_entity` exceeds `threshold_value`
- Smoothly transitions back when value drops below threshold

### Confirmation
- When `confirmation_enabled: true`, tapping the card shows a native HA dialog
- Action only executes if confirmed

---

## Notes

- Set unused sensor variables to `null` to hide them
- `threshold_entity` can be the same as `entity_value_1` or any other sensor
- All colors must be hex strings (e.g. `"#97BE5A"`)
