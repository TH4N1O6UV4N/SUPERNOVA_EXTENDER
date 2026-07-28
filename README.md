# 📡 SUPERNOVA EXTENDER

**A custom ESP8266 Wi-Fi Extender / Repeater firmware with a built-in mobile-friendly web dashboard, internet sharing (NAT), MAC-based firewall, and live network monitoring.**

Turn a cheap ESP8266 module (NodeMCU, Wemos D1 Mini, ESP-01/01S, or any generic ESP8266 board) into a fully manageable Wi-Fi range extender — no app install required, just connect and open a browser.

This project ships in **two editions**:

| Edition | Description |
|---|---|
| ⭐ **STAR** | The full-featured edition — asynchronous router connect (non-blocking, with live status polling), a dedicated Firewall panel with a live rule-count summary, and live-fetched current settings in the Settings panel |
| 💡 **LIGHT** | A lighter-weight edition — same visual design, but uses a simpler **synchronous (blocking)** router connect, a simpler "Block" panel (no live rule-count summary endpoint), a smaller EEPROM footprint (512 bytes vs 1024), and the Settings panel shows default placeholder values rather than the device's live current settings |

> Both editions share the same core Wi-Fi extender, NAT/NAPT sharing, MAC blocking, LED patterns, hard-reset button, auto-reconnect, Wi-Fi scanner, themes, sound, and client-tracking functionality. The differences are backend implementation details, not visual — see [Feature Differences by Edition](#feature-differences-by-edition) for the full breakdown.

---

## 👋 Welcome

<video src="https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Welcome.mp4" controls width="600"></video>

*A short introduction to the SUPERNOVA EXTENDER project.*

---

## 📸 Screenshots

### ⭐ STAR Edition

| Login Page | Main Dashboard |
|---|---|
| ![STAR Login Screen](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Screenshot_Star_Login.jpg) | ![STAR Main Dashboard](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Screenshot_Star_Mainpage.jpg) |

### 💡 LIGHT Edition

| Login Page | Main Dashboard |
|---|---|
| ![LIGHT Login Screen](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Screenshot_Light_Login.jpg) | ![LIGHT Main Dashboard](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Screenshot_Light_Mainpage.jpg) |

---

## 🎥 Demo Videos

### ⭐ STAR Edition Demo

<video src="https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/SUPERNOVA_EXTENDER_STAR_DEMO.mp4" controls width="600"></video>

**Fallback Link (Click to watch):**
[![Watch the STAR demo](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Screenshot_Star_Mainpage.jpg)](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/SUPERNOVA_EXTENDER_STAR_DEMO.mp4)

### 💡 LIGHT Edition Demo

<video src="https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/SUPERNOVA_EXTENDER_Light_DEMO.mp4" controls width="600"></video>

**Fallback Link (Click to watch):**
[![Watch the LIGHT demo](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/Screenshot_Light_Mainpage.jpg)](https://raw.githubusercontent.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/main/SUPERNOVA_EXTENDER_Light_DEMO.mp4)

---

## ✨ Features

- 🔌 **Wi-Fi Extender / Repeater** — connects to your existing router (STA mode) and rebroadcasts its own access point (AP mode) to extend coverage.
- 🌐 **Internet Sharing (NAT/NAPT)** — every device joined to the extender's Wi-Fi gets internet access through the router, exactly like a hardware repeater.
- 🔒 **Secure Web Dashboard** — cookie-session-based admin login protects every settings page.
- 📊 **Live Status Monitoring** — real-time uptime, signal strength (dBm/%), Wi-Fi channel, free heap memory, connected client count, and NTP-synced clock.
- 📶 **Wi-Fi Scanner** — lists nearby 2.4 GHz networks with signal bars and one-tap connect.
- 🔗 **Manual Connect** — type in an SSID/password directly (works for hidden networks too), with non-blocking async connection and live status polling.
- 🛡 **MAC Address Firewall** — block/unblock up to 5 devices by MAC address; blocked devices are auto-deauthenticated every 5 seconds.
- 👥 **Connected Client Tracker** — see every device on your extended network with its MAC address and connection duration.
- ⚙ **Configurable AP Settings** — change AP name, AP password, Open/WPA2 mode, and device IP address, all from the browser.
- 👤 **Configurable Admin Credentials** — change the dashboard login username/password anytime.
- 💾 **Persistent Settings (EEPROM)** — all configuration survives power loss and reboots.
- 🔄 **Auto-Reconnect** — automatically retries the router connection every 30 seconds if the link drops.
- 🔘 **Physical Hard-Reset Button** — hold the Flash/Boot button (GPIO0) for 5 seconds to wipe all settings and restart.
- 💡 **LED Status Patterns** — the onboard LED reports connection state at a glance (solid / fast blink / slow blink / rapid strobe).
- 🎨 **Cosmetic Extras** — multiple built-in UI themes and toggleable UI sound effects (STAR edition); simplified single theme (LIGHT edition).
- 📱 **Fully Responsive UI** — designed mobile-first; works great on a phone browser.

### 🔍 Feature Differences by Edition

| Aspect | ⭐ STAR | 💡 LIGHT |
|---|---|---|
| Router (Manual) Connect | Asynchronous — request returns immediately, page polls `/api/connectStatus` every 1.5s for the result | Synchronous — the request blocks for up to 15 seconds while the device attempts to connect; the web UI is unresponsive during this time |
| Device blocking/firewall panel | "Firewall" panel with block/unblock plus a live rule-count summary (`/api/firewall/status`, refreshes every 3s) | "Block" panel with block/unblock only; no live rule-count summary endpoint |
| Settings panel current values | Fetches and displays the device's actual current AP name/password/mode/IP via `/api/settings` | Shows hardcoded default placeholder values (`SUPERNOVA_EXTENDER`, `12345678`, `10.10.10.1`) regardless of what is actually saved |
| EEPROM size reserved | 1024 bytes | 512 bytes |
| Recommended use | Boards with more headroom (4 MB flash) where non-blocking UI responsiveness matters | Simpler/lower-resource setups, or where a smaller EEPROM footprint (1 MB flash boards) is preferred |

---

## 🧰 Hardware Requirements

| Item | Notes |
|---|---|
| ESP8266 board | NodeMCU, Wemos D1 Mini, ESP-01/01S, or any generic ESP8266 module |
| USB-to-serial adapter | Only needed for boards without an onboard USB port (e.g. bare ESP-01) |
| 2.4 GHz Wi-Fi router | ESP8266 hardware does not support 5 GHz networks |
| Flash size | 1 MB or 4 MB — see [Firmware Variants](#firmware-variants-1-mb-vs-4-mb) below |

---

## 📥 Downloads

The compiled firmware binaries are attached to the **[Releases](../../releases)** page of this repository.

| Edition | Flash Size | File |
|---|---|---|
| STAR | 1 MB (e.g. ESP-01/01S) | `SUPERNOVA_EXTENDER_STAR_1MB.bin` |
| STAR | 4 MB (e.g. NodeMCU / D1 Mini) | `SUPERNOVA_EXTENDER_STAR_4MB.bin` |
| LIGHT | 1 MB (e.g. ESP-01/01S) | `SUPERNOVA_EXTENDER_LIGHT_1MB.bin` |
| LIGHT | 4 MB (e.g. NodeMCU / D1 Mini) | `SUPERNOVA_EXTENDER_LIGHT_4MB.bin` |

> Only flash the 1 MB file to a genuine 1 MB chip, and the 4 MB file to a 4 MB chip — see [Firmware Variants](#firmware-variants-1-mb-vs-4-mb) below for how to tell them apart.

---

## 💾 Firmware Variants (1 MB vs 4 MB)

Each edition (STAR, LIGHT) is provided as two separate `.bin` files — one built for 1 MB flash boards, one for 4 MB flash boards:

| | 1 MB Flash (e.g. ESP-01/01S) | 4 MB Flash (e.g. NodeMCU / D1 Mini) |
|---|---|---|
| Flash Size setting used at build time | `1M` | `4M` |
| Flash Layout | `1M (no SPIFFS)` — this firmware only uses EEPROM | `4MB (FS:2MB OTA:~1019KB)` or similar |
| EEPROM usage | 1 KB (fits easily) | 1 KB (fits easily) |
| Best for | Small, low-cost extenders | General use — more headroom for scanning + NAT + web UI together |

---

## ⚡ Flashing the Firmware

1. Put the board into bootloader mode (automatic over USB for NodeMCU/D1 Mini; hold GPIO0 low on power-up for ESP-01).
2. Flash the matching `.bin` using `esptool.py`, Arduino IDE Upload, or a GUI flasher (NodeMCU PyFlasher / ESPHome Flasher). The **Flash Address is `0x00000000`** (the very start of flash) since this is a full, standalone firmware image:
   ```bash
   esptool.py --port COM5 --baud 115200 write_flash 0x00000000 firmware.bin
