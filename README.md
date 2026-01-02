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

## 🛡️ Stability & Fault Tolerance (UPS Integration)

Since the ESP32 is powered by a UPS and the PZEM modules are powered by the mains, a "communication hang" can occur during power transitions. This project implements a multi-tier watchdog system to ensure 24/7 reliability:

### 1. Smart Watchdog Logic
The system monitors the state of the PZEM sensors every minute. If data is missing (NaN or 0V) while the ESP32 is online, it performs the following recovery steps:
- **After 2 minutes of silence**: Triggers a **Soft UART Reset**. It flushes buffers and re-initializes the UART driver without rebooting the MCU.
- **After 5 minutes of silence**: Triggers a **Full Hardware Reboot** of the ESP32 to clear the Modbus stack and re-establish a fresh connection.

### 2. Enhanced UART Configuration
- **Increased RX Buffer**: Set to `512 bytes` to prevent buffer overflows during electrical noise caused by AC switching.
- **Manual Debugging**: A "Soft Reset UART" button is available in the web interface for manual maintenance.

### 3. Monitoring Accuracy (Riemann Sum & Utility Meter)
To ensure precise energy tracking in Home Assistant:
- **Integration**: Uses the `Riemann sum integral` (Left method) to convert raw Power (W) to Energy (kWh).
- **Weekly Tracking**: A `Utility Meter` helper is used for automated weekly resets, providing a reliable historical view even if the ESP32 reboots.

### 🔍 Troubleshooting
- **All values are 0 or NaN**: Check if 220V AC is connected to PZEM. The measurement chip is powered by the AC line, not the 5V UART line.
- **Watchdog Triggered**: If the log shows `Attempting soft UART reset`, it means electrical noise was detected on the data lines. Ensure GND of ESP32 and PZEM are common.
- **Frequency/Power Factor issues**: Ensure your PZEM-004T is version V3.0, as older versions do not support these registers.

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

Case:
<img width="887" height="502" alt="my screenshots 2025-12-31 at 20 07 46" src="https://github.com/user-attachments/assets/c8beb9c5-78d2-4e9c-b729-840210a1e2ec" />

<img width="887" height="502" alt="Screenshot 2026-01-02 at 10 04 01" src="https://github.com/user-attachments/assets/f58b00a1-964f-414b-bc61-637ffaa245d6" />

