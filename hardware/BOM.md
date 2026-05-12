# Bill of Materials

This document lists the main hardware and supporting materials used in the VR Gaming Glove project. The system is built as a low-cost academic prototype using an ESP32 microcontroller, five flex sensors, OpenGloves-compatible serial communication, and SteamVR pose tracking through the HTC Vive Tracker 3.0.

## Core Components

| Item | Description | Quantity | Notes |
|---|---|---:|---|
| ESP32 development board | Main embedded controller for sensor acquisition and USB serial communication | 1 | Reads flex sensors, filters data, normalizes values, and sends packets to the PC |
| Flex sensors | Finger-bend sensing elements | 5 | One sensor per finger |
| VR glove base | Wearable glove used as the mounting surface | 1 | Used to attach sensors, wiring, and tracker mount |
| HTC Vive Tracker 3.0 | 6-DoF hand position and orientation tracker | 1 | Mounted on the back of the glove |
| HTC SteamVR Base Station 1.0 | Lighthouse tracking source for Vive Tracker | 1 or more | Must provide clear line-of-sight during operation |
| Meta Quest 2 | VR headset used in PC Link mode | 1 | Used only as a display device in this project |
| PC or laptop | Host system for OpenGloves and SteamVR | 1 | Must support VR runtime and serial communication |

## Sensor Fabrication Materials

| Item | Description | Quantity | Notes |
|---|---|---:|---|
| Velostat | Pressure-sensitive conductive sheet used in flex sensor fabrication | As needed | Used as part of custom low-cost sensor construction |
| Copper tape | Conductive layer for sensor traces | As needed | Used in custom flex sensor assembly |
| Insulating layers | Non-conductive protective material | As needed | Helps isolate sensor layers and improve reliability |
| Jumper wires | Electrical connections between sensors and ESP32 | As needed | Used for breadboard or direct wiring |
| Resistors | Signal conditioning and analog input support | As needed | Required for stable analog readings |

## Assembly Materials

| Item | Description | Quantity | Notes |
|---|---|---:|---|
| Breadboard or perfboard | Prototyping board for sensor connections | 1 | Depends on assembly approach |
| Micro-USB cable | ESP32 power and data connection | 1 | Used for firmware upload and runtime serial communication |
| Adhesives | Tape, glue, or mounting material | As needed | Used to secure sensors and wiring |
| Protective glove layer | Base textile support | 1 | Improves wearability and protects components |
| Mounting plate or bracket | Tracker mounting support | 1 | Required to attach Vive Tracker securely |
| 3D-printed parts | Enclosures, guides, or holders | As needed | Used to organize wiring and support hardware placement |

## Software and Platform Dependencies

These are not physical parts, but they are required for the complete system:

- Arduino IDE
- ESP32 board support package
- OpenGloves
- SteamVR
- Meta Quest Link / Oculus PC software
- Windows 10 or Windows 11

## Estimated Cost Breakdown

The report defines the following estimated implementation budget:

| Category | Estimated Cost (SAR) | Notes |
|---|---:|---|
| Microcontroller | 80–100 | ESP32 board or similar control unit |
| Sensors | 250–300 | Flex sensors and related sensing hardware |
| 3D-printing materials | 50–80 | PLA and physical mounting parts |
| Wiring and connectors | 40 | Jumper wires, headers, solder, breadboards |
| Miscellaneous components | 40 | Cables, adhesives, protective materials |
| Contingency reserve | 40–50 | Spare parts and unexpected replacement needs |
| **Total** | **500–600** | Estimated project implementation budget |

## Notes
The BOM reflects the project’s design as a low-cost academic prototype rather than a commercial product. Some items, such as the VR-ready PC, Meta Quest 2, Vive Tracker, and Base Station, may be provided through laboratory or shared university resources instead of direct team purchase.