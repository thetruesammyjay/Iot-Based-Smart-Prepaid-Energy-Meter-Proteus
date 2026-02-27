# File Structure

## Project: Design and Implementation of an Advanced IoT-Based Smart Prepaid Energy Meter

---

```
Iot-Based-Smart-Prepaid-Energy-Meter-Proteus/
│
├── README.md                          # Project overview, setup guide, and simulation instructions
├── FILE-STRUCTURE.md                  # This document - annotated project directory layout
│
├── proteus/                           # All Proteus simulation design files
│   ├── SmartEnergyMeter.pdsprj        # Proteus 8 project file (opens the full simulation environment)
│   ├── SmartEnergyMeter.dsn           # Proteus schematic/circuit design file
│   ├── SmartEnergyMeter.hex           # Compiled firmware HEX file loaded into the virtual MCU
│   └── SmartEnergyMeter.lyt           # Proteus PCB layout file (optional, for PCB design view)
│
├── firmware/                          # Embedded C / Arduino source code for the microcontroller
│   ├── SmartEnergyMeter.ino           # Arduino IDE entry point (setup() and loop())
│   │
│   ├── src/                           # Modular source files — one module per peripheral/subsystem
│   │   ├── energy_measurement.h       # Header: energy data declarations (voltage, current, power, kWh from PZEM-004T)
│   │   ├── energy_measurement.cpp     # Implementation: PZEM-004T UART/Modbus polling and energy data extraction
│   │   │
│   │   ├── billing.h                  # Header: prepaid credit management declarations
│   │   ├── billing.cpp                # Implementation: credit deduction logic, tariff rates, low-balance alerts
│   │   │
│   │   ├── relay_control.h            # Header: relay switching declarations
│   │   ├── relay_control.cpp          # Implementation: load ON/OFF control based on credit status
│   │   │
│   │   ├── tft_display.h              # Header: TFT LCD display abstraction declarations
│   │   ├── tft_display.cpp            # Implementation: TFT_eSPI wrappers, colour screen layout rendering
│   │   │
│   │   ├── button_handler.h           # Header: push button input declarations
│   │   ├── button_handler.cpp         # Implementation: debounce, short/long press detection, recharge menu navigation
│   │   │
│   │   ├── wifi_module.h              # Header: ESP32 built-in Wi-Fi declarations
│   │   ├── wifi_module.cpp            # Implementation: WiFi.h / HTTPClient.h IoT data publish and remote recharge polling
│   │   │
│   │   ├── nvs_storage.h              # Header: non-volatile data storage declarations (ESP32 Preferences)
│   │   ├── nvs_storage.cpp            # Implementation: read/write credit balance and consumption logs to ESP32 NVS
│   │   │
│   │   └── token_validator.h          # Header: STS (Standard Transfer Specification) token validation
│   │       token_validator.cpp        # Implementation: token decryption, one-time-use enforcement
│   │
│   └── lib/                           # Third-party Arduino libraries used in this project
│       ├── TFT_eSPI/                  # TFT LCD driver library (ST7735/ILI9163 via SPI)
│       └── PZEM004Tv30/               # PZEM-004T v3.0 energy module UART/Modbus library
│
├── docs/                              # Technical documentation and reference material
│   ├── system-architecture.md         # High-level system block diagram description and data flow
│   ├── circuit-description.md         # Component-level explanation of the Proteus schematic
│   ├── component-list.md              # Bill of Materials (BOM) with part numbers and Proteus library names
│   ├── simulation-guide.md            # Step-by-step instructions for running the Proteus simulation
│   ├── billing-algorithm.md           # Mathematical derivation of energy measurement and billing logic
│   └── iot-integration.md             # IoT dashboard setup, API endpoints, and MQTT/HTTP protocol details
│
├── schematics/                        # Exported schematic and diagram image assets
│   ├── block-diagram.png              # Top-level system block diagram
│   ├── circuit-schematic.png          # Full Proteus circuit schematic export (high resolution)
│   ├── pcb-layout.png                 # PCB layout export (if applicable)
│   └── flowcharts/
│       ├── main-firmware-flow.png     # Firmware main loop flowchart
│       ├── billing-logic-flow.png     # Credit deduction and relay cutoff flowchart
│       └── token-recharge-flow.png    # Token entry, validation, and credit update flowchart
│
└── assets/                            # Supplementary media and reference assets
    ├── simulation-screenshots/        # Captured screenshots of the running Proteus simulation
    │   ├── normal-operation.png       # Meter displaying live readings under normal load
    │   ├── low-credit-alert.png       # LCD showing low-balance warning state
    │   ├── power-cutoff.png           # Relay open state on zero credit
    │   └── token-recharge.png         # Successful token entry and credit update
    └── datasheets/                    # Component datasheets for reference
        ├── PZEM-004T-datasheet.pdf    # Energy measurement module datasheet and Modbus protocol guide
        └── ESP32-datasheet.pdf        # ESP32 SoC technical reference and datasheet
```

---

## Directory Descriptions

### `proteus/`
Contains all files required to open and run the circuit simulation in Proteus Design Suite 8. The `.pdsprj` file is the primary entry point. The `.hex` file is the pre-compiled firmware binary that must be loaded into the virtual ESP32 microcontroller within the simulation. Any changes to the firmware source code require recompilation in the Arduino IDE (with the Espressif Arduino Core) to regenerate this HEX file.

### `firmware/`
Contains the full embedded software stack written for the ESP32 microcontroller using the Arduino framework. The code is organized into peripheral-specific modules under `src/` to promote separation of concerns, maintainability, and testability. Each module exposes a clean header interface and a corresponding implementation file.

### `docs/`
Contains all supplementary technical documentation. The `simulation-guide.md` is particularly important for first-time users setting up the Proteus environment. The `billing-algorithm.md` provides the formal mathematical basis for energy metering and credit calculations.

### `schematics/`
Exported static images of all circuit diagrams and system flowcharts. These are suitable for inclusion in the project report or presentation without requiring Proteus to be installed.

### `assets/`
Contains simulation screenshots demonstrating key system states and component datasheets used as hardware references during design.

---

## Key File Dependencies

| File | Depends On |
|---|---|
| `proteus/SmartEnergyMeter.pdsprj` | `proteus/SmartEnergyMeter.dsn`, `proteus/SmartEnergyMeter.hex` |
| `proteus/SmartEnergyMeter.hex` | All files under `firmware/src/` (compiled via Arduino IDE + Espressif Core) |
| `firmware/SmartEnergyMeter.ino` | All module headers in `firmware/src/` |
| `firmware/src/billing.cpp` | `energy_measurement.h`, `nvs_storage.h`, `relay_control.h` |
| `firmware/src/wifi_module.cpp` | `billing.h`, `token_validator.h` |
