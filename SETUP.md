# Setup Guide

This file explains how the VR Gaming Glove is assembled, connected, calibrated, and run.

## 1. Hardware setup

### 1.1 Build the finger sensors
Each finger sensor is made from:
- A strip of Velostat
- Two parallel strips of conductive copper tape
- Laminating sheet for structural support and insulation
- Jumper wires soldered at the exposed proximal end

The final laminated structure acts as a variable resistor. As the finger bends, resistance changes and the output voltage seen by the ESP32 changes accordingly.

### 1.2 Mount the sensors on the glove
- Use a heavy-duty work glove as the base.
- Mount one custom sensor per finger on the dorsal side.
- Use adjustable straps, thermoplastic adhesive, and 3D-printed guide channels to keep each sensor aligned.
- Add terminal clips near the fingertips to stop sensors from sliding during movement.

### 1.3 Install the ESP32 and wiring
The ESP32 is mounted on a breakout board. The back side includes a mini breadboard carrying the resistor network for the voltage divider.

Wiring logic:
- One terminal from each sensor goes to common GND.
- The ESP32 3.3 V supply feeds five fixed resistors.
- Each resistor is paired with one finger sensor in a voltage divider.
- The divider junction for each finger is connected to a different ADC-capable GPIO pin.

### 1.4 Install the tracker
Mount the HTC Vive Tracker 3.0 on the dorsal side above the electronics stack. This tracker provides the hand position and orientation, while the flex sensors provide finger curl.

## 2. Firmware setup

### Requirements
- Arduino IDE 2.x
- ESP32 board support package
- USB cable for flashing and serial communication

### Steps
1. Open the firmware project in Arduino IDE.
2. Select the correct ESP32 board and COM port.
3. Upload the firmware.
4. Open Serial Monitor at **115200 baud**.
5. Confirm that five sensor values are streamed continuously.

### Firmware processing loop
The ESP32 uses this real-time loop:
1. Read sensors
2. Filter
3. Normalize
4. Build packet
5. Transmit

## 3. PC software setup

### Requirements
- Windows 10 64-bit
- OpenGloves v0.3.x
- SteamVR 2.x
- Meta Quest 2 in Oculus Link mode
- HTC Vive Tracker 3.0
- Two SteamVR Base Station 1.0 units

### Steps
1. Connect the ESP32 to the PC with USB.
2. Start the two Base Station 1.0 units.
3. Power on the Vive Tracker 3.0.
4. Launch SteamVR.
5. Launch OpenGloves.
6. In OpenGloves, select the COM port for the ESP32.
7. Confirm five live sensor bars are updating.

## 4. Calibration
OpenGloves handles calibration at the middleware level rather than hard-coding it in firmware.

### Calibration process
1. Open the OpenGloves calibration interface.
2. Fully open the hand.
3. Fully bend each finger as requested.
4. Save the calibration profile.

Expected behavior:
- Flat state maps to approximately **0.00**.
- Bent state maps to approximately **1.00**.
- Mid-bend maps around **0.47–0.53**.

Observed user results:
- New users completed calibration in **18 s**, **22 s**, and **25 s**.
- Average calibration time: **21.7 s**.

## 5. SteamVR verification
After setup and calibration:
1. Open the SteamVR device panel.
2. Confirm the glove appears as a recognized input device.
3. Move the hand in all directions.
4. Bend each finger individually.
5. Check that virtual finger mapping matches the physical finger order.

Expected behavior:
- All five fingers move independently.
- Hand position and orientation follow the Vive Tracker.
- Finger articulation and hand pose are fused together without visible desynchronization.

## 6. Expected electrical readings
Typical values observed during testing:

| Finger state | Approx. voltage | Example ADC |
|---|---:|---:|
| Flat | 2.64–2.70 V | 3277–3350 |
| Bent | 1.32–1.57 V | 1638–1948 |

Thumb example:
- Fully flat: **3350** at **2.70 V**
- Fully bent: **1814** at **1.46 V**

## 7. Expected runtime performance
- End-to-end latency: **24–32 ms**
- Average measured latency: **27.2 ms**
- Battery endurance: **10 hours 23 minutes observed**
- Stable USB serial communication without dropouts during testing

## 8. Recommended repository attachments
To make the project reproducible, add these files:
- `hardware/assembly-process.md`
- `hardware/BOM.md`
- `hardware/circuit-diagrams/voltage-divider-schematic.png`
- `hardware/photos/figure-14-materials-used.jpg`
- `hardware/photos/figure-15-sensor-fabrication-stage.jpg`
- `hardware/photos/figure-16-completed-flex-sensor.jpg`
- `hardware/photos/figure-17-esp32-front-view.jpg`
- `hardware/photos/figure-18-esp32-rear-view.jpg`
- `hardware/photos/figure-19-glove-assembly-stage.jpg`
- `hardware/photos/figure-20-completed-glove-prototype.jpg`
- `docs/opengloves-calibration-screenshot.png`
- `docs/steamvr-hand-tracking-screenshot.png`

## 9. Troubleshooting
- If OpenGloves shows no sensor movement, verify the correct COM port is selected.
- If fingers move incorrectly, confirm packet order is thumb, index, middle, ring, little.
- If calibration feels inconsistent, repeat OpenGloves calibration after re-wearing the glove.
- If tracking is lost briefly, check Vive Tracker visibility relative to the Base Stations.
- If pinch detection feels weak, slightly reduce the pinch threshold in OpenGloves.
