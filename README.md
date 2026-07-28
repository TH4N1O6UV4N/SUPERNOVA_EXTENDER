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

<!-- Replace the src below with the CDN URL GitHub generates after you drag-and-drop Welcome.mp4 into the README editor on GitHub.com (see the Demo Videos section for the exact steps) -->
<video src="https://github.com/TH4N1O6UV4N/SUPERNOVA_EXTENDER/Welcome.mp4" controls width="600"></video>

*A short introduction to the SUPERNOVA EXTENDER project.*

> Place `Welcome.mp4` inside a `media/` folder at the repo root, then follow the steps in [Demo Videos](#demo-videos) to get a working play button on GitHub's main page.

---

## 📸 Screenshots

### ⭐ STAR Edition

| Login Page | Main Dashboard |
|---|---|
| ![STAR Login Screen](/Screenshot_Star_Login.jpg) | ![STAR Main Dashboard](/Screenshot_Star_Mainpage.jpg) |

### 💡 LIGHT Edition

| Login Page | Main Dashboard |
|---|---|
| ![LIGHT Login Screen](/Screenshot_Light_Login.jpg) | ![LIGHT Main Dashboard](/Screenshot_Light_Mainpage.jpg) |

> Place all four screenshot files inside a `screenshots/` folder at the repo root, using the exact filenames shown above, so they render correctly.

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

## 📁 Repository Structure

```
SUPERNOVA_EXTENDER/
├── bin/
│   ├── SUPERNOVA_EXTENDER_STAR_1MB.bin
│   ├── SUPERNOVA_EXTENDER_STAR_4MB.bin
│   ├── SUPERNOVA_EXTENDER_LIGHT_1MB.bin
│   └── SUPERNOVA_EXTENDER_LIGHT_4MB.bin
├── screenshots/
│   ├── Screenshot_Star_Login.jpg
│   ├── Screenshot_Star_Mainpage.jpg
│   ├── Screenshot_Light_Login.jpg
│   └── Screenshot_Light_Mainpage.jpg
├── media/
│   ├── Welcome.mp4
│   ├── SUPERNOVA_EXTENDER_STAR_DEMO.mp4
│   └── SUPERNOVA_EXTENDER_Light_DEMO.mp4
└── README.md
```

> This repository distributes **ready-to-flash `.bin` files only** — no `.ino` source code is published. Rename your existing `.bin` files to match the names above (or update the links in the [Downloads](#downloads) section below to match your actual filenames) before uploading.

---

## 📥 Downloads

The compiled firmware binaries are attached to the **[Releases](../../releases)** page of this repository (recommended), and/or available directly in the [`bin/`](bin/) folder above.

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
   ```
   esptool.py --port COM5 --baud 115200 write_flash 0x00000000 firmware.bin
   ```
   *(`0x0` and `0x00000000` refer to the exact same address — the leading zeros are optional — so either form works identically with esptool.)*
3. The device boots, creates its own Wi-Fi AP, and starts the web dashboard automatically.

**Flash address reference table (for GUI flashers such as NodeMCU PyFlasher / ESPHome Flasher):**

| Field | Value |
|---|---|
| Flash Address | `0x00000000` |
| File | The compiled `firmware.bin` (matching your board's flash size — 1 MB or 4 MB) |
| Flash Mode | `dout` (default for most ESP8266 boards) — use `dio`/`qio` only if your board specifically requires it |
| Flash Frequency | `40MHz` (default) |

> **Warning:** Only flash the 1 MB `.bin` to a genuine 1 MB chip, and the 4 MB `.bin` to a 4 MB chip. Flashing the wrong image can brick the boot process until re-flashed correctly.

---

## 🏁 First-Time Setup

1. Power on the device — it broadcasts **`SUPERNOVA_EXTENDER`** (default password `12345678`).
2. Connect your phone/PC to that network.
3. Browse to **`http://10.10.10.1`**.
4. Log in with the default admin credentials below, then go straight to **Settings** and change everything.

| Setting | Default |
|---|---|
| AP Name | `SUPERNOVA_EXTENDER` |
| AP Password | `12345678` |
| Device IP | `10.10.10.1` |
| Admin Username | `Admin` |
| Admin Password | `Password` |

---

## 🖥 Dashboard Guide

| Button | What it does |
|---|---|
| ⚙ **Settings** | Change AP name/password/mode, device IP, admin credentials, or factory-reset |
| 📊 **Status** | Live uptime, signal, connected devices, memory, NTP time |
| ✍ **Manual** | Manually enter a router SSID/password to connect (async in STAR; blocking for up to 15s in LIGHT) |
| 🔍 **Scan** | Lists nearby Wi-Fi networks, one-tap connect |
| 🛡 **Firewall** (STAR) / 🚫 **Block** (LIGHT) | Block/unblock devices by MAC address (STAR adds a live rule-count summary) |
| 🎨 **Theme** / 🔊 **Sound** | Cosmetic UI preferences |
| 📞 **Contact** | Developer contact link |
| 🚪 **Logout** | Ends the session |

**Physical controls (both editions):**
- **Hard Reset button (GPIO0):** hold 5 seconds to wipe all settings and restart.
- **Status LED:** Solid ON = connected; Fast blink = connecting; Slow blink = standalone AP; Rapid strobe = reset button held.

For the full step-by-step walkthrough of every feature, see the detailed **User Manual (PDF)** included in this repo/release.

---

## 🔌 Web API Reference

### ⭐ STAR Edition

| Endpoint | Purpose |
|---|---|
| `/` | Serves the dashboard |
| `/api/logincheck?user=&pass=` | Admin login |
| `/api/saveadmin?user=&pass=` | Update admin credentials |
| `/api/saveap?ap=&pw=&mode=` | Update AP name/password/mode |
| `/api/saveip?ip=` | Update device IP (restarts) |
| `/api/settings` | Get current AP settings (JSON) |
| `/api/scan` | Scan nearby networks |
| `/api/connect?ssid=&pass=` | Start async router connection |
| `/api/connectStatus` | Poll connection result |
| `/api/status` | Live device/network status |
| `/api/reset` | Factory reset |
| `/api/firewall/data` | Get MAC block-list |
| `/api/firewall/action?type=mac&act=block\|unblock&val=` | Block/unblock a MAC |
| `/api/firewall/status` | Firewall summary |

### 💡 LIGHT Edition

| Endpoint | Purpose |
|---|---|
| `/` | Serves the dashboard |
| `/api/logincheck?user=&pass=` | Admin login |
| `/api/saveadmin?user=&pass=` | Update admin credentials |
| `/api/saveap?ap=&pw=&mode=` | Update AP name/password/mode |
| `/api/saveip?ip=` | Update device IP (restarts) |
| `/api/scan` | Scan nearby networks |
| `/api/connect?ssid=&pass=` | Connect to a router (blocks for up to 15s until success/failure) |
| `/api/status` | Live device/network status |
| `/api/reset` | Factory reset |
| `/api/blocklist` | Get MAC block-list |
| `/api/block?mac=` | Block a MAC address |
| `/api/unblock?mac=` | Unblock a MAC address |

> Note: the LIGHT edition has no `/api/settings` (current-values fetch) or `/api/connectStatus` (async polling) endpoints — these are STAR-only.

All endpoints except login require a valid session cookie.

---

## 🔒 Security Recommendations

- 🔑 Change the default admin and AP passwords immediately after setup.
- 🔓 Avoid Open/Public AP mode unless intentionally hosting a free network.
- 🛡 Use the Firewall panel to remove unrecognized devices.
- 🔘 Keep physical access to the Flash button restricted (5-second hold = full reset).

---

## 🛠 Troubleshooting

| Symptom | Fix |
|---|---|
| Can't reach `10.10.10.1` | Make sure you're connected to the `SUPERNOVA_EXTENDER` Wi-Fi, not another network |
| LED stuck on fast blink | Wrong router password or out of range — re-enter via Manual Connect or Scan |
| No internet on extended network | Check Status panel — NAT only activates once the router link shows connected |
| Forgot admin password | Hold the Flash/Boot button 5 seconds to factory-reset |
| Device unresponsive after flashing | Re-flash with the `.bin` matching your board's actual flash size |

---

## 🤝 Contributing

Issues and pull requests are welcome. Please open an issue describing the board/flash size and edition (STAR or LIGHT) you're using along with any bug reports.

## 📄 License

Add your preferred license here (e.g. MIT) — no license is currently declared in this repository.

## 👤 Credits

Developed by **SUDHEESH.S**
📞 Contact: [Facebook](https://www.facebook.com/hitech.supernova)
