# ESP8266 Matrix Clock with Web Config & OTA

A highly customizable, Wi-Fi connected digital clock using an ESP8266 (NodeMCU) and a MAX7219 LED Matrix. Features a "Slot Machine" animation for seconds, a built-in web interface for settings, and Over-The-Air (OTA) firmware updates via GitHub.

## 🌟 Features

* **Accurate Time:** Syncs automatically with NTP servers (pool.ntp.org).
* **Web Dashboard:** Configure settings from your phone or PC browser (no recoding needed!).
    * Set Brightness (0-15).
    * Toggle 12/24 Hour format.
    * Select Time Zone (Presets for India, US, UK, etc.).
* **Visual Effects:**
    * "Slot Machine" scroll animation for Seconds.
    * Clean, readable system font.
* **OTA Updates:** Automatically checks this GitHub repository for updates and installs them.
* **Persistent Memory:** Settings are saved to EEPROM (Power-off protection).
* **Wi-Fi Manager:** Connects to known Wi-Fi or creates a Hotspot ("MatrixClock") for initial setup.

## 🛠️ Hardware Required

1.  **ESP8266 NodeMCU** (or Wemos D1 Mini)
2.  **MAX7219 LED Dot Matrix Module** (4-in-1 FC-16 type)
3.  Jumper Wires (Female-to-Female)
4.  Micro-USB Cable

## 🔌 Wiring

| NodeMCU (ESP8266) | MAX7219 Matrix |
| :--- | :--- |
| **3V3** (or Vin) | VCC |
| **GND** | GND |
| **D7** (GPIO 13) | DIN (Data In) |
| **D4** (GPIO 2) | CS (Chip Select) |
| **D5** (GPIO 14) | CLK (Clock) |

> **Note:** If your display shows mirrored/garbage text, change `HARDWARE_TYPE` in `main.cpp` from `FC16_HW` to `GENERIC_HW`.

## 📦 Libraries Used

Ensure these libraries are installed in PlatformIO (`platformio.ini`) or Arduino IDE:

* `MD_Parola` (by MajicDesigns)
* `MD_MAX72XX` (by MajicDesigns)
* `NTPClient` (by Fabrice Weinberg)
* `WiFiManager` (by tzapu)
* `ESP8266WiFi` (Built-in)
* `ESP8266WebServer` (Built-in)
* `EEPROM` (Built-in)

## ⚙️ Installation & OTA Setup

1.  **Clone the Repo:**
    ```bash
    git clone [https://github.com/rohanshirodker/ESP-Clock.git](https://github.com/rohanshirodker/ESP-Clock.git)
    ```
2.  **Edit GitHub Links:**
    Open `main.cpp` and locate:
    ```cpp
    const char* URL_VERSION = "[https://raw.githubusercontent.com/YOUR_USER/YOUR_REPO/main/version.txt](https://raw.githubusercontent.com/YOUR_USER/YOUR_REPO/main/version.txt)";
    const char* URL_FIRMWARE = "[https://raw.githubusercontent.com/YOUR_USER/YOUR_REPO/main/firmware.bin](https://raw.githubusercontent.com/YOUR_USER/YOUR_REPO/main/firmware.bin)";
    ```
    Replace these with your **RAW** GitHub file URLs.

3.  **Compile & Upload:**
    Flash the code to your ESP8266.

4.  **First Run:**
    * The clock will display "WiFi".
    * Connect your phone to the Hotspot named **"MatrixClock"**.
    * A portal will open. Select your home Wi-Fi and enter the password.

5.  **Access Settings:**
    * Once connected, the clock briefly shows the last digit of its IP address (e.g., `.45`).
    * Open a browser and go to `http://192.168.X.45` (replace with actual IP).
    * Adjust Brightness, Time Zone, and Format. Click **Save**.

## 🚀 How to Push an OTA Update

To update clocks remotely without plugging them in:

1.  Increase the `#define CURRENT_VERSION "1.3"` in your code (e.g., to "1.4").
2.  Compile the code to generate a binary file (`firmware.bin`).
    * *PlatformIO:* `.pio/build/nodemcuv2/firmware.bin`
    * *Arduino:* Sketch -> Export Compiled Binary.
3.  Update the `version.txt` file in your repo to match the new number ("1.4").
4.  Commit and Push both `firmware.bin` and `version.txt` to GitHub.
5.  Restart the clock. It will see the new version and update itself!

## ⚠️ Troubleshooting

* **Gibberish Text?**
    Go to `main.cpp` and change:
    `#define HARDWARE_TYPE MD_MAX72XX::FC16_HW`
    to
    `#define HARDWARE_TYPE MD_MAX72XX::GENERIC_HW`

* **Settings Wiped after Update?**
    Do not change the order of variables in the `struct Settings` in the code. New variables must be added at the end.

---
**Current Version:** 1.3
