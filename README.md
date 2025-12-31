# Dual-Phase Energy Monitor (XIAO ESP32-C3 + 2x PZEM-004T)

IoT solution based on ESPHome for real-time monitoring of two electrical phases using a Seeed Studio XIAO ESP32-C3 and two PZEM-004T modules.

## Features
- **Dual-Phase Monitoring**: Independent tracking of Voltage, Power, and Energy for two lines.
- **Smart Filtering**: Automatic removal of "garbage" data (spikes) during boot or power loss.
- **Zero-Value Logic**: Sensors automatically show `0` instead of `NA` when AC power is disconnected.
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


🛠 How to Change PZEM-004T Address

By default, all PZEM modules come with address 1. To use two modules on the same UART bus, one must be changed to address 2.

⚠️ Safety Warning

High Voltage: You must connect 220V AC to the PZEM input terminals. The address change command will not be saved if the measurement side is not powered by AC.

One at a time: Disconnect all other PZEM modules. Only the one you are configuring should be connected to the ESP32.

Steps:

Connect the first PZEM module to the Seeed Studio XIAO ESP32-C3.

Connect 220V AC to the PZEM terminals.

Flash the pzem_address_changer.yaml to the ESP32.

Open the ESPHome Web Dashboard or logs.

Click the "Set PZEM Address to 2" button.

Look for PZEM_FIX: Command to set address 2 sent! in the logs.

Important: Power cycle the PZEM (disconnect AC and 5V for 10 seconds) to apply the changes.

Now you can connect both modules in parallel and use the main energy-monitor.yaml configuration.
