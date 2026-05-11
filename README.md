# VR Gaming Glove

A low-cost VR glove prototype that combines five custom Velostat flex sensors for finger tracking, an ESP32 microcontroller for signal acquisition, OpenGloves for middleware integration, and an HTC Vive Tracker 3.0 for 6-DoF hand pose tracking. The project was developed for **CS 521 - Project Implementation, Term 2 - 2026** at the College of Computer Science and Information Technology, Imam Abdulrahman Bin Faisal University.

## Team
- Mohammed Al-Zayer — 2220006453
- Ali Al-Jama — 2220004743
- Ahmed Al Khamis — 2220002883
- Khaled Kishkish — 2220001795
- Jawad Al-Deabel — 2220006577

**Supervisor:** Dr. Tareq Alkhaldi

## Project overview
The final system integrates three tracking layers:
- **Finger tracking:** five custom Velostat-based flex sensors connected to an ESP32.
- **Hand pose tracking:** HTC Vive Tracker 3.0.
- **VR runtime:** OpenGloves + SteamVR.

The Meta Quest 2 is used only as a PC-connected display device, while spatial tracking is handled by SteamVR Base Station 1.0 units.

## Repository structure
```text
vr-gaming-glove/
├── README.md
├── SETUP.md
├── LICENSE
├── .gitignore
├── firmware/                          ← rename from vrgloves-firmware/
│   ├── AdvancedConfig.h
│   ├── Encoding.ino
│   ├── ICommunication.ino
│   ├── SerialBTCommunication.ino
│   ├── SerialCommunication.ino
│   ├── _main.ino
│   ├── gesture.ino
│   ├── haptics.ino
│   ├── input.ino
│   ├── lucidgloves-firmware.ino
│   └── README.md
├── hardware/
│   ├── assembly-process.md
│   ├── BOM.md
│   └── photos/                        ← upload your 5 photos here
├── docs/
│   └── Chapter 5 Implementation and Testing.docx   ← delete the Draft version
└── tests/
    └── test-results-summary.md
```

## Hardware implementation
The glove is built on heavy-duty work gloves and uses five custom-made flex sensors fabricated from Velostat sandwiched between copper tape strips and enclosed in laminating sheets. These sensors act as variable resistors whose resistance changes with bending, allowing the system to detect individual finger motion.

The sensors are connected to an ESP32 through a five-channel voltage divider network. One terminal of each sensor is connected to common ground, while the divider junction for each finger is routed to a unique ESP32 ADC input. The ESP32 reads analog values, applies lightweight filtering, normalizes them, and transmits structured serial packets to the PC over USB at 115200 baud.

## Software pipeline
1. ESP32 reads five analog channels.
2. Firmware filters the raw values using lightweight smoothing.
3. Values are normalized into a bend range.
4. Five-value serial packets are sent over USB.
5. OpenGloves parses the serial data and performs calibration.
6. Vive Tracker 3.0 provides 6-DoF hand pose.
7. SteamVR renders the final virtual hand and interaction state.

## Assembly evidence
The repository should include a full hardware assembly record in `hardware/assembly-process.md` and the corresponding photos in `hardware/photos/`.

## Assembly Photos
| | |
|---|---|
| ![Fig14](hardware/photos/figure-14-materials-used.jpg) | ![Fig15](hardware/photos/figure-15-sensor-fabrication-stage.jpg) |
| Materials used | Sensor fabrication stage |
| ![Fig16](hardware/photos/figure-16-completed-flex-sensor.jpg) | ![Fig17](hardware/photos/figure-17-esp32-front-view.jpg) |
| Completed flex sensor | ESP32 front view |
| ![Fig18](hardware/photos/figure-18-esp32-rear-view.jpg) | ![Fig19](hardware/photos/figure-19-glove-assembly-stage.jpg) |
| ESP32 rear – voltage divider | Glove assembly stage |
| ![Fig20](hardware/photos/figure-20-completed-glove-prototype.jpg) | |
| Completed glove prototype | |

## Key benchmark results
- Active finger channels: **5 / 5**
- SteamVR device registration: **Successful**
- Skeletal hand animation: **Real-time**
- End-to-end latency: **24–32 ms**
- Battery endurance: **10 hours observed**
- Connection stability: **No dropouts observed**
- Overall testing result: **21 / 21 test cases passed**

## Test execution summary
| Test level | Total | Passed | Failed | Pending |
|---|---:|---:|---:|---:|
| Unit Testing | 6 | 6 | 0 | 0 |
| Integration Testing | 5 | 5 | 0 | 0 |
| System Testing | 7 | 7 | 0 | 0 |
| User Acceptance Testing | 3 | 3 | 0 | 0 |
| **Total** | **21** | **21** | **0** | **0** |

## Notable measured results
- Flat thumb ADC: 3350 at 2.70 V
- Bent thumb ADC: 1814 at 1.46 V
- Flat finger voltages: 2.64–2.70 V
- Bent finger voltages: 1.32–1.57 V
- Average measured latency across repetitions: 27.2 ms
- Calibration time for new users: 18 s, 22 s, and 25 s (average 21.7 s)

## Future improvements
- Fine-tune pinch gesture sensitivity in OpenGloves.
- Reduce wrist stiffness caused by tracker weight.
- Improve resistance to temporary tracker occlusion.
- Consider lighter mechanical mounting revisions.
