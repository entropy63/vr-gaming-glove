# VR Gaming Glove

A low-cost VR glove prototype developed at Imam Abdulrahman Bin Faisal University for **CS 511 / CS 521**. The system combines five custom flex sensors, an ESP32 microcontroller, OpenGloves middleware, and an HTC Vive Tracker 3.0 to provide real-time finger tracking and 6-DoF hand pose inside SteamVR.

## Team
- Mohammed Abbas Al-Zayer — 2220006453
- Ali Ahmed Al-Jama — 2220004743
- Ahmed Hussain Al Khamis — 2220002883
- Jawad Ali Al-Deabel — 2220006577
- Khaled Ali Kishkish — 2220001795

**Supervisor:** Dr. Tareq M. Alkhaldi

## Project Overview
The VR Gaming Glove is designed as a low-cost alternative to traditional VR controllers and expensive commercial data gloves. Instead of relying on buttons and handheld controller input, the system captures finger bending and hand movement directly, then maps that input into a SteamVR-compatible virtual hand.

The final system is built around four layers:

- **Finger tracking:** five flex sensors mounted on the glove, one per finger
- **Embedded processing:** ESP32 reads, filters, normalizes, and transmits sensor values
- **Pose tracking:** HTC Vive Tracker 3.0 provides 6-DoF hand position and orientation
- **VR integration:** OpenGloves merges finger data with tracker pose and sends it to SteamVR

The Meta Quest 2 is used only as a **PC-connected display device** through Link mode. It is not used for tracking in this project.

## Implementation Focus
The project is implementation-driven and centers on the construction of a wearable embedded VR input device. The glove integrates sensing hardware, firmware, middleware, and VR runtime support into one real-time interaction pipeline.

The implementation includes:

- custom flex sensor fabrication
- analog signal acquisition using ESP32
- lightweight filtering and normalization
- USB serial communication to the PC
- OpenGloves calibration and skeletal mapping
- SteamVR integration for hand rendering and interaction

## Hardware Implementation

### Flex Sensors
The glove uses five custom-made flex sensors, one for each finger. These sensors are fabricated using low-cost materials such as Velostat, copper tape, and insulating layers. Each sensor behaves as a variable resistor, changing its electrical response when the finger bends.

### Embedded Controller
An ESP32 microcontroller is used as the core embedded processing unit. It reads analog values from the five sensors, performs lightweight processing, and transmits structured finger-bend data to the PC over USB serial.

### Pose Tracking
To provide 6-DoF hand tracking, the glove includes an HTC Vive Tracker 3.0 mounted on the back of the hand. Spatial tracking is handled through SteamVR Base Station 1.0 Lighthouse tracking.

### VR Display
The VR scene is displayed through a Meta Quest 2 headset connected to the PC. In this project, the headset is used only as a display device and does not contribute tracking data.

## Firmware Implementation
The ESP32 firmware is responsible for the core embedded processing tasks of the glove. Its responsibilities include:

- initializing ADC and serial interfaces
- reading raw analog flex sensor values
- applying lightweight smoothing to reduce noise
- normalizing finger values into a standard bend range
- building structured serial packets
- transmitting packets to the PC at a steady update rate

The firmware follows a simple real-time loop:

1. Read sensor values from all five fingers
2. Filter the raw values
3. Normalize the processed values
4. Build a packet in fixed finger order
5. Send the packet over USB serial

This design keeps the firmware lightweight, modular, and suitable for real-time operation on the ESP32.

## Communication Pipeline
The communication path used in the final design is:

Flex Sensors → ESP32 → USB Serial → OpenGloves → SteamVR → Meta Quest 2 Display

Finger-bend data is transmitted from the ESP32 to the PC using USB serial communication at **115200 baud or higher**. OpenGloves receives the packet stream, parses the finger values, applies calibration, and exposes the glove as a SteamVR-compatible input device.

## OpenGloves Integration
OpenGloves is used as the main middleware layer between the glove hardware and SteamVR. This avoids the need to build a custom OpenVR driver.

OpenGloves is responsible for:

- receiving serial data from the ESP32
- parsing five finger channels
- storing calibration values
- mapping finger values to SteamVR skeletal hand input
- merging finger tracking with Vive Tracker pose data
- enabling gesture-based interaction through SteamVR

This middleware-based design reduces technical complexity and improves compatibility with existing VR software.

## Calibration
Calibration is performed through the OpenGloves interface rather than being permanently hard-coded into firmware. This allows the glove to adapt to different users and sensor tolerances without reflashing the ESP32.

The calibration workflow allows the user to:

- set minimum and maximum finger bend values
- preview live finger motion
- verify connection and tracking status
- refine finger mapping for better skeletal animation

## SteamVR Integration
SteamVR acts as the runtime environment for rendering the virtual hand and handling VR interactions. In the final architecture:

- ESP32 provides finger-bend values
- Vive Tracker 3.0 provides position and orientation
- OpenGloves merges both streams
- SteamVR renders the final articulated hand model

This produces a hybrid tracking system where finger shape comes from the glove and hand pose comes from the tracker.

## Functional Features
The system is designed to support the following implementation features:

- five-finger bend tracking
- real-time hand pose tracking
- finger calibration through OpenGloves
- skeletal hand animation in SteamVR
- gesture support such as open hand, fist, pinch, and pointing
- interaction with VR objects in compatible SteamVR environments
- real-time debug and monitoring through software tools

## Design Constraints
The implementation is shaped by several practical constraints:

- ESP32 has limited memory and processing power
- communication must remain compatible with OpenGloves
- no custom OpenVR driver is used
- SteamVR skeletal input rules must be followed
- Quest 2 tracking is not used
- all components must remain low-cost and safe for wearable use
- total system latency must remain suitable for natural VR interaction

These constraints influenced both firmware design and hardware architecture.

## Repository Structure
    vr-gaming-glove/
    ├── README.md
    ├── SETUP.md
    ├── LICENSE
    ├── .gitignore
    ├── firmware/
    │   ├── esp32-glove.ino
    │   └── README.md
    ├── hardware/
    │   ├── assembly-process.md
    │   ├── BOM.md
    │   ├── photos/
    │   ├── circuit-diagrams/
    │   └── 3d-models/
    ├── docs/
    │   └── CS-511-VR-Gaming-Glove-Project-Final-Report.pdf
    └── tests/
        └── test-results-summary.md

## Development Workflow
The project workflow can be summarized as follows:

1. Fabricate the flex sensors
2. Mount the sensors onto the glove
3. Connect the sensors to the ESP32
4. Program the ESP32 firmware
5. Stream normalized finger data over USB serial
6. Launch OpenGloves and calibrate each finger
7. Attach and configure the Vive Tracker 3.0
8. Run the glove inside SteamVR
9. Test virtual hand movement and interaction

## Testing Focus
The testing approach for the implementation is divided into four stages:

- **Component testing:** verify sensors, ESP32 reading, and serial transmission
- **Integration testing:** verify ESP32, OpenGloves, and SteamVR communication
- **System testing:** verify full VR glove operation inside SteamVR
- **User experience testing:** verify usability, comfort, and responsiveness

Key testing concerns include:

- sensor accuracy
- signal stability
- communication reliability
- tracking continuity
- calibration quality
- VR interaction correctness
- comfort during use

## Future Improvements
The design supports future extension and improvement. Possible next steps include:

- wireless communication
- haptic feedback
- dual-hand support
- improved gesture recognition
- lighter mechanical mounting
- improved sensor durability
- broader SteamVR binding support
- enhanced comfort for long sessions

## Summary
The VR Gaming Glove demonstrates a practical implementation of a low-cost, SteamVR-compatible hand-tracking device using affordable embedded hardware and open-source middleware. By combining custom flex sensors, ESP32 firmware, OpenGloves, and Vive Tracker-based pose tracking, the system provides a modular foundation for natural VR hand interaction in academic and experimental environments.