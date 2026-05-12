# Performance Benchmarks

This document summarizes the measured performance of the VR Gaming Glove prototype during implementation and testing. It focuses on the key benchmark areas used to validate the system: finger tracking, sensor readings, latency, power consumption, battery endurance, connection stability, and overall SteamVR integration.

## Benchmark Scope

The benchmark process evaluated the glove as a complete real-time input pipeline:

- Flex sensors detect finger bending
- ESP32 reads and processes five analog channels
- USB serial sends normalized finger values to the PC
- OpenGloves parses and calibrates the incoming data
- SteamVR merges finger data with Vive Tracker 3.0 hand pose
- Meta Quest 2 displays the final VR scene in PC Link mode

The measured benchmark categories included:

- functional operation
- per-finger sensor voltage and ADC values
- power consumption
- battery endurance
- end-to-end latency
- connection stability
- SteamVR device registration

## Test Environment

The benchmarking setup used the following environment:

| Item | Configuration |
|---|---|
| Host PC | Windows 10 64-bit, AMD Ryzen 7, 32 GB RAM, NVIDIA RTX 4070 Super |
| Firmware IDE | Arduino IDE 2.x with ESP32 board support |
| Middleware | OpenGloves v0.3.x |
| VR Runtime | SteamVR 2.x |
| Headset | Meta Quest 2 in PC Link mode |
| Tracking Hardware | HTC Vive Tracker 3.0, SteamVR Base Station 1.0 x2 |
| Glove Hardware | 5 custom Velostat flex sensors, ESP32 breakout board, 5 x 10 kΩ resistors |

## Core Results

The final prototype met the project’s primary real-time performance goals. All five fingers were tracked independently, the glove registered successfully in SteamVR through OpenGloves, and the system maintained stable operation without dropouts during testing.

| Metric | Target | Achieved | Status |
|---|---|---|---|
| Active finger channels | 5 | 5 | Pass |
| SteamVR device registration | Successful | Successful | Pass |
| Skeletal hand animation | Real-time | Real-time | Pass |
| Battery endurance | 8 hours minimum | 10 hours 23 minutes | Pass |
| End-to-end latency | Less than 50 ms | 24-31 ms, average 27.2 ms | Pass |
| Connection stability | No dropouts | No dropouts observed | Pass |
| Finger normalization range | 0.0-1.0 | 0.0-1.0 | Pass |

## Functional Performance

Functional testing confirmed that all five finger channels responded separately and were normalized into a consistent bend range. OpenGloves recognized the glove as a valid input device without requiring a custom OpenVR driver, and skeletal hand animation worked in real time when finger data was fused with Vive Tracker pose data.

Verified functional outcomes:

- thumb, index, middle, ring, and little fingers all responded independently
- normalized bend output worked correctly from 0.0 to 1.0 and 0 to 100 representations
- OpenGloves registered the glove successfully in SteamVR
- finger articulation and hand pose produced a realistic virtual hand
- long-duration testing showed no packet loss or serial disconnections

## Sensor Voltage Benchmarks

Each flex sensor was connected as the lower leg of a voltage divider using a fixed 10 kΩ pull-up resistor and the ESP32 3.3 V rail. The ESP32 ADC converted the resulting voltages into 12-bit raw values from 0 to 4095.

### Per-finger sensor readings

| Finger | Flat Resistance | Flat Voltage | Flat ADC | Bent Resistance | Bent Voltage | Bent ADC |
|---|---:|---:|---:|---:|---:|---:|
| Thumb | 45 kΩ | 2.70 V | 3350 | 8 kΩ | 1.46 V | 1814 |
| Index | 43 kΩ | 2.68 V | 3326 | 8 kΩ | 1.46 V | 1814 |
| Middle | 44 kΩ | 2.69 V | 3338 | 9 kΩ | 1.57 V | 1948 |
| Ring | 42 kΩ | 2.67 V | 3314 | 8 kΩ | 1.46 V | 1814 |
| Little | 40 kΩ | 2.64 V | 3277 | 7 kΩ | 1.32 V | 1638 |

Notes:

- resistance values are approximate and specific to the custom Velostat sensors
- calibration min/max values were stored in OpenGloves
- all measured ADC pin voltages remained within safe ESP32 limits

## Filtering and Signal Quality

Signal conditioning improved measurement stability before transmission to the PC. Exponential smoothing reduced static jitter significantly while preserving responsive finger motion.

Measured filtering results:

- raw ADC standard deviation: 18.4 counts
- filtered ADC standard deviation: 4.2 counts
- jitter reduction: 77%
- maximum filtered deviation from mean: 9 counts

Cross-channel interference was also negligible during isolated finger testing:

- single-finger bends changed the intended channel by about 1200-1600 ADC counts
- adjacent channels changed by at most 12 ADC counts

## Power Consumption

The main power draw came from the ESP32 running in active CPU mode with USB serial communication enabled and wireless radios disabled. The Vive Tracker 3.0 used its own internal battery and was not part of the ESP32 subsystem power budget.

### Component-level power estimates

| Component | Operating Mode | Supply Voltage | Estimated Current | Estimated Power |
|---|---|---:|---:|---:|
| ESP32 | Active CPU, USB serial, no radio | 3.3 V | 80-100 mA | 264-330 mW |
| Sensor divider network | Continuous analog read | 3.3 V | 5-8 mA | 16-26 mW |
| USB serial interface | 115200 baud | 5.0 V | 5 mA | 25 mW |
| Total ESP32 subsystem | Combined | 3.3-5.0 V | 90-110 mA | 300-360 mW |
| Vive Tracker 3.0 | Lighthouse tracking active | 3.7 V internal battery | 120 mA | 444 mW |

## Battery Endurance

The ESP32 subsystem was tested using a 1000 mAh LiPo battery. Observed runtime exceeded both the theoretical minimum target and the project requirement.

| Battery Capacity | Average Current | Calculated Endurance | Observed Endurance | Status |
|---:|---:|---:|---:|---|
| 1000 mAh | 95 mA | 10.5 hours | 10 hours 23 minutes | Pass |

Battery benchmark notes:

- the system target was at least 8 hours
- observed runtime exceeded the target comfortably
- no packet loss or interruption was observed before battery depletion

## Latency Benchmarks

End-to-end latency was measured across the full motion pipeline, from finger bend to visible virtual hand response. The largest contributors were USB serial transmission and the SteamVR frame pipeline.

### Pipeline latency breakdown

| Pipeline Stage | Responsible Component | Estimated Latency |
|---|---|---:|
| ADC sampling, 5 channels | ESP32 firmware | 0.5-1 ms |
| Exponential smoothing | ESP32 firmware | 0.5 ms |
| Packet build and serial transmission | ESP32 + USB | 8-12 ms |
| OpenGloves parse and finger mapping | PC middleware | 2-4 ms |
| Vive Tracker pose acquisition | SteamVR Lighthouse | 1-2 ms |
| Pose fusion | OpenGloves + SteamVR | 1-2 ms |
| SteamVR skeletal render pipeline | SteamVR runtime at 90 Hz | 11 ms per frame |
| Total end-to-end estimated | Full pipeline | 24-32 ms |

### Measured latency trials

| Trial | Measured Latency |
|---|---:|
| 1 | 24 ms |
| 2 | 27 ms |
| 3 | 31 ms |
| 4 | 28 ms |
| 5 | 26 ms |

Latency summary:

- average measured latency: 27.2 ms
- maximum observed latency: 31 ms
- system requirement: less than 50 ms
- result: pass

## Integration Stability

Integration benchmarks confirmed that the glove worked reliably across ESP32, OpenGloves, SteamVR, and Vive Tracker subsystems.

Validated integration behavior:

- OpenGloves detected the ESP32 serial stream successfully
- all five physical fingers mapped to the correct virtual fingers
- Vive Tracker pose tracking showed accurate translation and rotation
- finger data and pose data were merged without visible desynchronization
- USB reconnect recovery completed automatically within 6 seconds
- SteamVR displayed the glove as `OpenGloves Controller Left Hand`

## Gesture and Interaction Results

Gesture tests verified the practical usability of the glove inside SteamVR scenes. The system supported open hand, fist, pinch, and point gestures through OpenGloves gesture logic.

Measured gesture and interaction results:

- open hand: 10/10 correct detections
- fist/grab: 10/10 correct detections
- pinch: 9/10 on first attempt, passed on re-test
- point: 10/10 correct detections
- overall first-pass result: 39/40
- overall re-test result: 40/40
- false positives: 0

Virtual object interaction also worked correctly in all five grab-and-release trials.

## Comfort and Calibration Benchmarks

User acceptance testing showed that the glove was practical for short VR sessions and easy to calibrate. Calibration remained within the project usability target, and comfort stayed acceptable through 60 minutes of wear.

### Calibration time

| Subject | Time |
|---|---:|
| User 1 | 18 s |
| User 2 | 22 s |
| User 3 | 25 s |
| **Average** | **21.7 s** |

### Comfort ratings

| Time Point | Ratings | Average |
|---|---|---:|
| 30 minutes | 4, 4, 3 | 3.7 |
| 60 minutes | 4, 3, 3 | 3.3 |

### Interaction accuracy perception

| Task | Average Rating |
|---|---:|
| Grab | 4.0 |
| Place | 3.7 |
| Point | 4.3 |
| Pinch | 3.3 |
| Wave | 4.0 |
| **Overall average** | **3.86** |

## Observations

All benchmarked categories passed, but a few small issues were noted for future improvement:

- one pinch gesture was missed initially due to slightly slow finger closure
- one 1-second tracker occlusion occurred when the hand moved behind the user’s body
- one subject reported mild wrist stiffness after about 55 minutes because of tracker weight

These observations did not cause test failure, but they highlight useful areas for future hardware and gesture-tuning improvements.