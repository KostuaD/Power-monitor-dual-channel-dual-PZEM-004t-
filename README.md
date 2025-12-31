# Dual-Phase Energy Monitor (XIAO ESP32-C3 + 2x PZEM-004T)

IoT solution based on ESPHome for real-time monitoring of two electrical phases using a Seeed Studio XIAO ESP32-C3 and two PZEM-004T modules.

## Features
- **Dual-Phase Monitoring**: Independent tracking of Voltage, Power, and Energy for two lines.
- **Smart Filtering**: Automatic removal of "garbage" data (spikes) during boot or power loss.
- **Zero-Value Logic**: Sensors automatically show `0` instead of `NA` when AC power is disconnected.
- **Weekly Tracker**: Automated weekly energy consumption calculation.
- **Web Interface**: Built-in dashboard for real-time viewing and energy counter resets.

## Hardware Connection
- **MCU**: Seeed Studio XIAO ESP32-C3
- **Sensors**: 2x PZEM-004T (V3.0)
- **UART Pins**: TX (GPIO21 / D6), RX (GPIO20 / D7)
- **Important**: Modules must have unique Modbus addresses (1 and 2).

## Installation
1. Install [ESPHome](https://esphome.io/).
2. Create a `secrets.yaml` file with your WiFi and API credentials.
3. Use the provided `.yaml` configuration to flash your device.
4. If you have a new PZEM module, use the "Reset" and "Address change" functions to set it up.

## Caution
**HIGH VOLTAGE!** Working with AC 220V is dangerous. Ensure all connections are isolated and the device is housed in a non-conductive enclosure.
