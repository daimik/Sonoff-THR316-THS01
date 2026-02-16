# 🌡️ Sonoff THR316 + THS01 — ESPHome Thermostat

> **Turn a cheap Sonoff relay into a smart thermostat!** This ESPHome config flashes a Sonoff THR316 with a THS01 (SI7021) temperature & humidity sensor and turns it into a fully controllable climate device in Home Assistant. 🏠✨

---

## 🤔 What Is This?

A ready-to-use **ESPHome YAML configuration** that transforms your Sonoff THR316 into a smart thermostat. Originally built to keep a barn at a safe temperature during winter, but it works great for any space that needs simple heat control! 🐴🥶

---

## ✨ Features

| Feature | Details |
|---|---|
| 🌡️ **Temperature & Humidity** | Reads from the THS01 (SI7021) sensor every 60 seconds |
| 🔥 **Thermostat Control** | Automatic heat on/off with configurable target temperature (5–15 °C) |
| 💡 **LED Indicators** | Power LED, green LED, and Wi-Fi status LED — all managed automatically |
| 🔘 **Physical Button** | Toggle the relay manually using the onboard button (GPIO0) |
| 🔄 **Restart Button** | Restart the device remotely from Home Assistant |
| 📶 **Wi-Fi Diagnostics** | Signal strength (RSSI), IP address, and MAC address exposed as sensors |
| ⏱️ **Uptime Tracking** | Human-readable uptime (e.g. `3d 14h 22m 8s`) |
| 🛡️ **Fallback Hotspot** | If Wi-Fi fails, the device creates its own AP so you can reconfigure it |
| 🔒 **Encrypted API** | Secure communication with Home Assistant |
| 📡 **OTA Updates** | Update firmware wirelessly — no USB cable needed after initial flash |

---

## 🏗️ Hardware You'll Need

| Component | Model |
|---|---|
| 🔌 **Smart Relay** | [Sonoff THR316](https://sonoff.tech/en-nl/products/sonoff-th-origin-smart-temperature-and-humidity-monitoring-switch?_pos=2&_psq=Sonoff+THR316&_ss=e&_v=1.0) |
| 🌡️ **Sensor** | [Sonoff THS01](https://sonoff.tech/en-nl/products/sonoff-ths01-temp-and-humi-sensor?_pos=1&_psq=Sonoff+THS01&_ss=e&_v=1.0) |

---

## 📋 GPIO Pinout Reference

| GPIO | Function |
|---|---|
| `GPIO0` | Onboard button (pull-up, inverted) |
| `GPIO21` | Relay output |
| `GPIO25` | THS01 sensor data pin |
| `GPIO27` | Sensor power switch |
| `GPIO13` | Green LED (inverted) |
| `GPIO15` | Wi-Fi status LED (inverted) |
| `GPIO16` | Power LED (inverted) |

---

## 🚀 Getting Started

### 1️⃣ Prerequisites

- 🏠 [Home Assistant](https://www.home-assistant.io/) up and running
- ⚡ [ESPHome](https://esphome.io/) add-on installed
- 🔌 Sonoff THR316 + THS01 sensor

### 2️⃣ Configure the YAML

Open `barn-thermostat.yaml` and update the following:

```yaml
# Set your API encryption key
api:
  encryption:
    key: "your-encryption-key-here"

# Set your OTA password
ota:
  - platform: esphome
    password: "your-ota-password"

# Fallback hotspot password
wifi:
  ap:
    password: "your-fallback-password"
```

> 💡 **Tip:** Wi-Fi credentials use `!secret` — make sure your `secrets.yaml` has `wifi_ssid` and `wifi_password` defined.

### 3️⃣ Flash the Device

```bash
# First time — flash via USB
esphome run barn-thermostat.yaml

# After that — flash over the air! 🎉
esphome run barn-thermostat.yaml --device barn-thermostat.local
```

### 4️⃣ Add to Home Assistant

Once flashed, the device should auto-discover in Home Assistant under **Settings → Devices & Services → ESPHome**. Accept the integration and you're done! 🎉

---

## 🔥 Thermostat Behavior

The climate entity uses these settings out of the box:

| Setting | Value |
|---|---|
| 🌡️ Min temperature | 5 °C |
| 🌡️ Max temperature | 15 °C |
| 🎯 Default target | 8 °C |
| 🎚️ Step size | 0.1 °C |
| ⏳ Min heating run time | 5 minutes |
| ⏳ Min heating off time | 5 minutes |
| ⏳ Min idle time | 30 seconds |

> 🔧 These values are tuned for frost protection in an outbuilding. Adjust the `min_temperature`, `max_temperature`, and `default_target_temperature_low` in the YAML to match your use case!

---

## 📡 Exposed Entities in Home Assistant

### Sensors
- 🌡️ Temperature
- 💧 Humidity
- 📶 Wi-Fi RSSI
- ⏱️ Uptime

### Controls
- 🔌 Relay switch (on/off)
- 🔥 Climate thermostat
- 🔄 Restart button

### Diagnostics
- 🌐 IP Address
- 🏷️ MAC Address
- 📦 ESPHome Version
- 📡 Connection Status

---

## ⚠️ Important Notes

- The config uses an **external DHT component** from [Beungoud/esphome](https://github.com/Beungoud/esphome/tree/fix-esp32-dht-SI7021) to fix SI7021 compatibility on ESP32. This is pulled automatically during compilation.
- The sensor power pin (`GPIO27`) is turned on at boot — the THS01 needs this to operate.
- The framework is set to `esp-idf`. If you run into issues, you can switch to `arduino` in the YAML.

---

## 🤝 Contributing

Found a bug or want to adapt this for a different Sonoff device? Feel free to open an issue or submit a PR — contributions are welcome! 💜

---

## 📄 License

This project is open source. Use it, modify it, make it yours! 🛠️
