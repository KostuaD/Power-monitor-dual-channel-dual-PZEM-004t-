# ⚡ Power Monitor Dual Channel (PZEM-004T + XIAO ESP32-C3)

A reliable energy monitoring solution for two independent electrical lines (phases) based on ESPHome. This project is specifically designed for stability under unstable power grid conditions and works seamlessly with UPS-backed controllers.



## ✨ Features (v26.01.02.4)
- **Dual-Phase Support**: Separate metrics for each line + calculated Total Power.
- **UPS-Aware**: Intelligent Watchdog restores communication if 220V mains fail while the ESP32 remains powered.
- **Multi-Tier Protection**:
  - **Soft UART Reset**: Automatically clears the data bus after 2 minutes of silence.
  - **Hard Reboot**: Performs a full MCU reboot after 5 minutes of persistent communication failure.
- **Full Metrics**: Voltage, Power, Energy, Frequency (Hz), and Power Factor (PF).
- **Advanced Diagnostics**: Service reset buttons and real-time connectivity status directly in Home Assistant.

## 🛠 Hardware
- **Controller**: [Seeed Studio XIAO ESP32-C3](https://www.seeedstudio.com/Seeed-Studio-XIAO-ESP32C3-p-5431.html).
- **Sensors**: 2x PZEM-004T V3.0.
- **Powering**: ESP32 is powered via UPS (5V USB), while PZEM modules are powered by the measured AC 220V line.

### Pinout Diagram
| XIAO ESP32-C3 | PZEM-004T (Module 1 & 2) | Description |
| :--- | :--- | :--- |
| **5V** | **VCC** | Logic Power |
| **GND** | **GND** | Common Ground |
| **D6 (GPIO21)** | **RX** | Data Transmission (TX on ESP) |
| **D7 (GPIO20)** | **TX** | Data Reception (RX on ESP) |

> **Note:** PZEM modules must be connected in parallel to the same UART bus on the ESP32.



## ⚙️ PZEM Address Configuration
To run two modules on a single bus, you must assign unique Modbus addresses (default is `1` for all units).

1. Connect **only one** PZEM module to the ESP32.
2. Apply 220V AC to the PZEM (the measurement chip requires AC power to save settings).
3. Flash the `utils/pzem_address_fixer.yaml` file.
4. Click the **"Set Address to 2"** button in the web interface.
5. Repeat for the second module if you wish to use address `3`.
6. Once configured, connect both modules in parallel and flash the main configuration.

## 🏠 Home Assistant Integration
For accurate weekly/monthly energy tracking, it is recommended to use built-in Helpers:

1. **Riemann Sum Integral**:
   - Input: `Power Monitor 01 Total Power`
   - Integration Method: `Left`
   - Result: `Total Energy (kWh)`
2. **Utility Meter**:
   - Input: Your Riemann Integral sensor.
   - Reset Cycle: `Weekly` / `Monthly`.



## 📝 Troubleshooting
- **Status "Disconnected"**: Ensure 220V AC is applied to the PZEM module (the logic won't respond without AC power) and check for a common GND.
- **Values 0 or NaN**: The Watchdog will attempt to reset the UART automatically. Check logs for `watchdog: Attempting soft UART reset`.
- **Compilation Errors**: Ensure you are using the latest version of ESPHome and the `esp-idf` framework.

## 📄 License
This project is released under the MIT License. Feel free to use and modify it as you see fit.
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

