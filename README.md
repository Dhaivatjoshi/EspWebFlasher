# ⚡ ESP Web Flasher

**Flash ESP32 firmware directly from your browser** using a dynamic web interface powered by [esp-web-tools](https://github.com/esphome/esp-web-tools).
This tool allows users to **select from multiple firmware projects**, then flash the selected firmware onto their ESP32 device — no software installation required.

[🔗 Live Flash Page](https://dhaivatjoshi.github.io/EspWebFlasher/)

---

## 📚 Table of Contents

* [⚡ ESP Web Flasher](#-esp-web-flasher)
* [How It Works](#-how-it-works)
* [How to Use](#-how-to-use)
* [Repo Structure](#-repo-structure)
* [Creating manifest.json](#-creating-manifestjson)

  * [Structure of manifest.json](#structure-of-manifestjson)
  * [Where to place manifest.json](#where-to-place-manifestjson)
* [Technologies Used](#-technologies-used)
* [Hosting](#-hosting)
* [Security Notes](#-security-notes)
* [License](#-license)

---

## 🛠 How It Works

This web app automatically:

* Loads available firmware projects from this repository.
* Detects folders containing a `manifest.json` file (required by `esp-web-tools`).
* Lists the valid firmware targets in a dropdown.
* Provides a **WebUSB-based installer button** to flash the selected firmware.

Built with the [ESP Web Tools JavaScript module](https://github.com/esphome/esp-web-tools).

---

## 🛠 How to Use

1. Connect your **ESP32** to your computer via USB.
2. Visit the [Web Flasher Page](https://dhaivatjoshi.github.io/EspWebFlasher/).
3. Select your desired project from the dropdown.
4. Click **Install**.
5. Select your ESP32’s **COM port** (if prompted).
6. When prompted, **press and hold the BOOT button** on the ESP32, then confirm flashing.
7. Wait for upload to complete — that’s it!

> ℹ️ You must use **Google Chrome, Edge, Brave, or Chromium-based browsers** that support WebUSB.

---

## 📁 Repo Structure

Firmware builds (with `manifest.json`) should follow this pattern:

```
EspWebFlasher/
├── multiSensorHat/
│   ├── build/
│   │   ├── bootloader/
│   │   ├── partition_table/
│   │   ├── Example1/
│   │   │   ├── firmware.bin
│   │   │   ├── manifest.json
│   │   ├── Example2/
│   │   │   ├── firmware.bin
│   │   │   ├── manifest.json
```

The site scans:

* Root-level folders
* `multiSensorHat/examples/*` subfolders

Any folder with a valid `manifest.json` is listed in the dropdown for flashing.

---

## 📄 Example manifest.json

Here’s a minimal working `manifest.json` to flash ESP32:

```json
{
  "name": "MultiSensorHat Firmware",
  "version": "1.7",
  "builds": [
    {
      "chipFamily": "ESP32",
      "parts": [
        { "path": "bootloader/bootloader.bin", "offset": 4096 },
        { "path": "partition_table/partition-table.bin", "offset": 32768 },
        { "path": "firmware.bin", "offset": 65536 }
      ]
    }
  ]
}
```

Make sure the binary files are placed correctly relative to the manifest path.

---

## 📄 Creating `manifest.json`

To flash firmware via [ESP Web Tools](https://github.com/esphome/esp-web-tools), each firmware must include a **`manifest.json`** describing what files to flash and at what memory offsets.

---

### ✅ **Structure of `manifest.json`**

Here is a valid example for ESP32 firmware:

```json
{
  "name": "MultiSensorHat Firmware",
  "version": "1.7",
  "builds": [
    {
      "chipFamily": "ESP32",
      "parts": [
        { "path": "bootloader/bootloader.bin", "offset": 4096 },
        { "path": "partition_table/partition-table.bin", "offset": 32768 },
        { "path": "firmware.bin", "offset": 65536 }
      ]
    }
  ]
}
```

### 🔧 Explanation:

| Field        | Description                                                        |
| ------------ | ------------------------------------------------------------------ |
| `name`       | Friendly name shown on the flasher UI.                             |
| `version`    | Version of the firmware.                                           |
| `chipFamily` | Must match your target chip: `ESP32`, `ESP8266`, `ESP32-C3`, etc.  |
| `path`       | Path to each binary file relative to the `manifest.json` location. |
| `offset`     | Flash offset in bytes. These values are typical for ESP32.         |

---

### 📂 **Where to place `manifest.json`**

Place the `manifest.json` in the **same folder as your `.bin` files**. The expected structure looks like this:

```
EspWebFlasher/
├── multiSensorHat/
│   └── build/
│       └── examples/
│           └── I2C_to_Serial_Example/
│               ├── firmware.bin
│               ├── bootloader/
│               │   └── bootloader.bin
│               ├── partition_table/
│               │   └── partition-table.bin
│               └── manifest.json   <- goes here
```

📝 **Tip**: Make sure the `path` fields inside `manifest.json` correctly reference the binary files **relative to the manifest’s folder**.

---

###  Multiple Firmware Projects?

You can create separate folders for each firmware variation (e.g. different sensor bridges or configurations), each with their own `manifest.json`. The web flasher will automatically detect and list them.

---

## Technologies Used

* [ESP Web Tools](https://github.com/esphome/esp-web-tools)
* GitHub Pages (`gh-pages` branch or `main`)
* GitHub REST API for loading content dynamically
* WebUSB for device communication

---

## 📦 Hosting

This web tool is static — it's fully hosted via GitHub Pages.
To deploy your own:

1. Clone this repo.
2. Modify the HTML and manifest logic as needed.
3. Enable **GitHub Pages** on your repository.

---
