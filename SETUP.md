# Setup Guide

## 1. Hardware Setup

### 1.1 Build the flex sensors
- Velostat strip sandwiched between two copper tape strips
- Enclosed in laminating sheet for insulation
- Jumper wires soldered at exposed proximal end

### 1.2 Mount sensors on the glove
- Heavy-duty work glove as base
- One sensor per finger on dorsal side
- Secured with straps, thermoplastic adhesive, 3D-printed guide channels
- Fingertip terminal clips to prevent sensor sliding

### 1.3 Wire the ESP32
- One terminal from each sensor → shared GND
- ESP32 3.3 V → five fixed 10 kΩ resistors (rear breadboard)
- Each resistor-sensor junction → unique ADC GPIO pin

### 1.4 Mount the Vive Tracker
- HTC Vive Tracker 3.0 on dorsal side above the electronics stack

---

## 2. Firmware Setup

Requirements: Arduino IDE 2.x, ESP32 board support, USB cable

1. Open `firmware/esp32-glove.ino` in Arduino IDE
2. Select correct ESP32 board and COM port
3. Click Upload
4. Open Serial Monitor at **115200 baud**
5. Confirm 5 sensor values streaming

---

## 3. PC Software Setup

Requirements: Windows 10, SteamVR 2.x, OpenGloves v0.3.x, Meta Quest 2 (Oculus Link), HTC Vive Tracker 3.0, 2x SteamVR Base Station 1.0

1. Connect ESP32 via USB
2. Power on both Base Stations
3. Power on Vive Tracker
4. Launch SteamVR
5. Launch OpenGloves → select ESP32 COM port
6. Confirm 5 live sensor bars responding

---

## 4. Calibration

1. Open OpenGloves calibration interface
2. Fully open hand → maps to 0.00
3. Fully bend each finger when prompted → maps to 1.00
4. Save calibration profile

Average calibration time: ~22 seconds

---

## 5. Expected Electrical Readings

| Finger | State | Voltage | ADC Raw |
|---|---|---|---|
| Thumb | Flat | 2.70 V | 3350 |
| Thumb | Bent | 1.46 V | 1814 |
| Index | Flat | 2.68 V | 3326 |
| Index | Bent | 1.46 V | 1814 |
| Middle | Flat | 2.69 V | 3338 |
| Middle | Bent | 1.57 V | 1948 |
| Ring | Flat | 2.67 V | 3314 |
| Ring | Bent | 1.46 V | 1814 |
| Little | Flat | 2.64 V | 3277 |
| Little | Bent | 1.32 V | 1638 |

---

## 6. Troubleshooting

| Problem | Fix |
|---|---|
| No sensor movement in OpenGloves | Check correct COM port |
| Wrong finger mapping | Verify order: thumb, index, middle, ring, little |
| Inconsistent calibration | Re-wear glove and redo calibration |
| Tracking lost briefly | Check Vive Tracker visibility to Base Stations |
