ESP32RET
=======

Reverse Engineering Tool running on ESP32 based hardware. Supports both EVTV ESP32 and Macchina A0

There is a precompiled binary version of this program here:
https://www.savvycan.com/ESP32RET_Updater.zip

## Changes Made in This Fork

This fork includes several improvements and modifications to make the ESP32RET more robust and easier to use:

1. **PlatformIO Compatibility**: Made the project compatible with PlatformIO for easier compilation and dependency management
2. **CAN Bus Pin Configuration**: Changed CAN bus pins to GPIO 16 (RX) and GPIO 17 (TX) ESP32WROOM32 hardware compatibility
3. **Conservative Default Settings**: Changed startup configuration to:
   - Default CAN speed: 250k baud (was 500k) - 250k is more common in heavy machinery and trucks, while 500k is more common on light vehicles.
   - Default mode: Listen-only (was normal transmit) - prevents interference with existing CAN traffic in case of wrong speed selected by the default (before connecting to gvret).
4. **Unique WiFi SSID**: WiFi hotspot name now includes the device MAC address (format: `sniffer_aabbccddeeff`) to avoid conflicts when multiple devices are nearby
 4. **Reduced buffer size for WiFi and Serial**: Connection was previously working for 3-4 seconds then dropping, reducing the buffer made it more stable. 


#### Requirements:

You will need the following to be able to compile the run this project:

- [Arduino IDE](https://www.arduino.cc/en/Main/Software) Tested on 1.8.13
- [Arduino-ESP32](https://github.com/espressif/arduino-esp32) - Allows for programming the ESP32 with the Arduino IDE
- [esp32_can](https://github.com/collin80/esp32_can) - A CAN library that supports the built-in CAN
- [esp32_mcp2517fd](https://github.com/collin80/esp32_mcp2517fd) - CAN library supporting MCP2517FD chips
- [can_common](https://github.com/collin80/can_common) - Common structures and functionality for CAN libraries

PLEASE NOTE: The Macchina A0 uses a WRover ESP32 module which includes PSRAM. But, do NOT use the WRover
board in the Arduino IDE nor try to enable PSRAM. Doing so causes a fatal crash bug.

The EVTV board has no PSRAM anyway.

This program is larger than the default partitioning scheme. You will need to use
a larger scheme. The recommended way to do this: Tools -> Partition Scheme -> Minimal SPIFFS

By default a wifi hotspot will be created by this firmware. The SSID is either ESP32RETSSID (for EVTV board) or
A0RETSSID (For Macchina A0). The default WPA2 password is "aBigSecret" (Minus the quote marks) You can configure
different settings from the serial port created when connected to USB. The serial port is 1 Megabit speed.

All libraries belong in %USERPROFILE%\Documents\Arduino\hardware\esp32\libraries (Windows) or ~/Arduino/hardware/esp32/libraries (Linux/Mac).

The canbus is supposed to be terminated on both ends of the bus. This should not be a problem as this firmware will be used to reverse engineer existing buses. However, do note that CAN buses should have a resistance from CAN_H to CAN_L of 60 ohms. This is affected by placing a 120 ohm resistor on both sides of the bus. If the bus resistance is not fairly close to 60 ohms then you may run into trouble.

#### The firmware is a work in progress. What works:
- CAN0 / CAN1 reading and writing
- Preferences are saved and loaded
- Text console is active (configuration and CAN capture display)
- Can connect as a GVRET device with SavvyCAN
- LAWICEL support (somewhat tested. Still experimental)
- Bluetooth works to create an ELM327 compatible interface (tested with Torque app)

#### What does not work:
- Digital and Analog I/O

#### License:

This software is MIT licensed:

Copyright (c) 2014-2020 Collin Kidder, Michael Neuweiler

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

