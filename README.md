# 🕒 ESP8266 WiFi Matrix Clock

A precise, internet-synchronized digital clock built with an **ESP8266 (NodeMCU)** and a **MAX7219 LED Dot Matrix** display. It features automatic time syncing via NTP, a web-based configuration portal, and Over-the-Air (OTA) updates.

## ✨ Features

  * **Internet Time:** Automatically syncs with NTP servers for perfect accuracy.
  * **Web Config Portal:** Easily configure WiFi, brightness, time zone, and 24h/12h format from your phone or PC.
  * **Dual-Zone Layout:** Optimized display with hours/minutes on the left and seconds on the right for a tight, seamless look.
  * **OTA Updates:** Update firmware wirelessly via a GitHub repository.
  * **WiFi Manager:** No hardcoding WiFi credentials; connects to a captive portal for easy setup.
  * **Clean Look:** Uses the system font with optimized spacing to fit HH MM SS on a 32-pixel wide display.

## 🛠️ Hardware Requirements

  * **ESP8266 Board:** NodeMCU v2 (or Wemos D1 Mini)
  * **Display:** MAX7219 Dot Matrix Module (4-in-1 FC-16 Hardware)
  * **Power:** Micro-USB cable and 5V power source.
  * **Wires:** Female-to-Female jumper wires.

### Wiring Diagram

| MAX7219 Pin | NodeMCU (ESP8266) Pin |
| :--- | :--- |
| **VCC** | **VV** (or 3V3, but 5V is brighter) |
| **GND** | **GND** |
| **DIN** | **D7** (GPIO 13) |
| **CS** | **D4** (GPIO 2) |
| **CLK** | **D5** (GPIO 14) |

> **Note:** The `CS` pin is configurable in the code (`#define CS_PIN D4`), but `DIN` and `CLK` are fixed hardware SPI pins on the ESP8266.

## 📦 Software Installation

### 1\. Prerequisites

  * **VS Code** with **PlatformIO** extension installed.

### 2\. Project Setup

1.  Create a new PlatformIO project for the board **NodeMCU 1.0 (ESP-12E Module)**.
2.  Open the `platformio.ini` file and paste the following configuration:

<!-- end list -->

```ini
[env:nodemcuv2]
platform = espressif8266
board = nodemcuv2
framework = arduino
upload_speed = 921600
monitor_speed = 115200

lib_deps =
    majicdesigns/MD_MAX72XX @ ^3.3.1
    majicdesigns/MD_Parola @ ^3.7.1
    ropg/ezTime @ ^0.8.3
    tzapu/WiFiManager @ ^0.16.0
    bblanchon/ArduinoJson @ ^6.21.3
```

3.  Copy the provided `main.cpp` code into your project's `src/main.cpp` file.

### 3\. OTA Configuration (Optional)

To enable wireless updates, edit the lines at the top of `main.cpp` with your own GitHub repository links:

```cpp
#define CURRENT_VERSION "1.4"
const char* URL_VERSION = "https://raw.githubusercontent.com/YOUR_USER/REPO/main/version.txt";
const char* URL_FIRMWARE = "https://raw.githubusercontent.com/YOUR_USER/REPO/main/firmware.bin";
```

### 4\. Flash Filesystem

This project uses `LittleFS` to save your settings.

1.  In VS Code, click the **PlatformIO Alien Icon**.
2.  Go to **Project Tasks** -\> **nodemcuv2** -\> **Platform**.
3.  Click **Build Filesystem Image** and then **Upload Filesystem Image**.

### 5\. Upload Code

1.  Connect your ESP8266 via USB.
2.  Click the **Arrow Icon (→)** in the bottom bar to build and upload the firmware.

## 📱 How to Use

### First Time Setup

1.  Power on the clock. It will display **"WiFi..."**.
2.  On your phone or PC, look for a WiFi network named **"SmartClock"**. Connect to it.
3.  A captive portal should open automatically (or go to `192.168.4.1`).
4.  Select your home WiFi network and enter the password.
5.  The clock will save, reboot, and display the time.

### Changing Settings

1.  Find the IP address of your clock (check your router, or look at the Serial Monitor during boot).
2.  Open that IP address in a web browser (e.g., `http://192.168.1.50`).
3.  You can adjust:
      * **Brightness:** Slider from 0 to 15.
      * **Time Zone:** Select your region.
      * **Format:** Toggle 12h / 24h.

## ❓ Troubleshooting

**Display shows garbage or random dots?**

  * This is usually a hardware type mismatch.
  * In `main.cpp`, try changing `HARDWARE_TYPE` to `MD_MAX72XX::GENERIC_HW` or `MD_MAX72XX::ICSTATION_HW`.
  * Ensure your `DIN` is connected to `D7` and `CLK` to `D5`.

**Display is upside down or mirrored?**

  * You may need to rotate the display in software or physically flip the modules.
  * If using `GENERIC_HW`, you often need to rotate the modules 90 degrees physically or adjust the library transform.

**Stuck on "WiFi..."?**

  * The ESP cannot connect to the saved network.
  * It will eventually timeout and restart the Access Point "SmartClock" so you can re-enter credentials.
