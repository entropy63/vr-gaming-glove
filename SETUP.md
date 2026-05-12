# Setup Guide

This guide explains how to set up the VR Gaming Glove system for testing and use with SteamVR. The system combines five flex sensors, an ESP32 microcontroller, OpenGloves middleware, an HTC Vive Tracker 3.0, SteamVR Base Station 1.0 tracking, and a Meta Quest 2 headset running in PC Link mode.

## System Requirements

### Hardware
- ESP32 development board
- Five flex sensors
- Glove base
- HTC Vive Tracker 3.0
- HTC SteamVR Base Station 1.0 setup
- Meta Quest 2 headset
- VR-ready Windows PC
- Micro-USB cable for ESP32
- Mounting hardware or 3D-printed tracker mount
- Wiring, resistors, connectors, and breadboard or soldered connections

### Software
- Arduino IDE
- ESP32 board support package
- SteamVR
- OpenGloves
- Meta Quest Link / Oculus PC software
- USB serial drivers if required by the ESP32 board

## System Architecture
The runtime data path is:

Flex Sensors → ESP32 → USB Serial → OpenGloves → SteamVR → Meta Quest 2 Display

The ESP32 handles finger sensing and packet transmission. The Vive Tracker handles 6-DoF hand pose. OpenGloves merges both data streams and exposes the glove as a SteamVR-compatible input device.

## Before You Start
Before powering the system, verify the following:

- all five flex sensors are physically mounted to the glove
- each sensor is wired correctly to the ESP32 analog input circuit
- the ESP32 can be powered safely over USB
- the Vive Tracker is mounted securely on the back of the glove
- the Base Station has clear line-of-sight to the tracker
- the Quest 2 is available for PC Link mode
- SteamVR and OpenGloves are installed on the PC

## Step 1: Prepare the Hardware

### 1. Mount the Flex Sensors
Attach one flex sensor to each finger of the glove. Make sure the sensors are aligned with finger bending direction and do not shift during movement.

### 2. Connect the Sensors to the ESP32
Wire all five flex sensors to the ESP32 analog input interface. The report design expects the ESP32 to read all five sensors through analog pins and provide stable 3.3V-regulated sensor operation.

### 3. Mount the Vive Tracker
Attach the HTC Vive Tracker 3.0 to the back of the glove using a secure mount. Keep the tracker stable so that pose alignment remains consistent during movement.

### 4. Connect the ESP32 to the PC
Use a Micro-USB cable to connect the ESP32 to the PC. This connection provides both power and serial data transfer.

## Step 2: Flash the ESP32 Firmware

### 1. Open the Firmware Project
Open the firmware source in Arduino IDE.

### 2. Select the Board
Choose the correct ESP32 board from the Arduino IDE board menu.

### 3. Select the Port
Choose the correct serial port for the ESP32.

### 4. Upload the Firmware
Compile and upload the firmware to the board.

### 5. Verify Serial Output
Open the Serial Monitor or Serial Plotter and confirm that sensor data is being transmitted. The design expects structured packets containing five finger values in a fixed order.

Expected finger order:

Thumb, Index, Middle, Ring, Pinky

The packet format is expected to be comma-separated values, for example:

0.02,0.15,0.78,0.63,0.10

## Step 3: Prepare the PC Software

### 1. Install SteamVR
Install SteamVR on the Windows PC and confirm it launches correctly.

### 2. Install OpenGloves
Install OpenGloves and confirm that the application or driver components are available on the PC.

### 3. Install Quest Link Software
Install Meta Quest Link / Oculus PC software so the Quest 2 can be used in PC-tethered mode.

### 4. Confirm ESP32 Serial Access
Make sure the PC can see the ESP32 serial device. If needed, install the required USB serial driver for the board.

## Step 4: Connect the Quest 2

### 1. Enable PC Link Mode
Connect the Meta Quest 2 to the PC and enable Link mode.

### 2. Confirm Display Function
The headset should act only as a display device for the VR environment. In this project, Quest tracking and controller input are not used as the main tracking source.

## Step 5: Set Up SteamVR Tracking

### 1. Place the Base Station
Install the HTC SteamVR Base Station 1.0 in a position with clear, unobstructed line-of-sight to the glove-mounted Vive Tracker.

### 2. Power the Tracking Hardware
Turn on the Base Station and the Vive Tracker.

### 3. Pair the Vive Tracker
Pair the Vive Tracker with SteamVR and verify that it is detected correctly.

### 4. Check Tracking Stability
Move the glove in the tracked area and confirm that SteamVR reports stable tracker pose data.

## Step 6: Start OpenGloves

### 1. Launch OpenGloves
Open OpenGloves on the PC.

### 2. Select the Serial Device
Choose the serial port connected to the ESP32.

### 3. Match the Communication Settings
Configure OpenGloves to read the incoming serial packet format expected from the ESP32 firmware. The report design uses USB serial communication at 115200 baud or higher.

### 4. Confirm Live Input
Verify that OpenGloves receives five finger channels and displays changing values when you bend your fingers.

## Step 7: Calibrate the Glove

### 1. Open the Calibration Interface
Use the OpenGloves calibration menu.

### 2. Calibrate Open-Hand Position
Hold your hand open and register the minimum bend position for each finger.

### 3. Calibrate Closed-Hand Position
Close your hand into a fist and register the maximum bend position for each finger.

### 4. Verify Finger Preview
Use the OpenGloves preview window to confirm that the virtual finger motion matches your real finger motion.

### 5. Adjust If Needed
Repeat calibration if finger movement appears too weak, too strong, inverted, or unstable.

## Step 8: Verify SteamVR Integration

### 1. Launch SteamVR
Start SteamVR after OpenGloves is active and the Vive Tracker is connected.

### 2. Confirm Device Recognition
Check that the glove is recognized as a tracked VR input device through OpenGloves.

### 3. Verify Hand Rendering
Move your hand and bend your fingers. The virtual hand should update with both:
- tracker-based hand pose
- sensor-based finger curl

### 4. Test Basic Gestures
Verify supported gestures such as:
- open hand
- fist
- pinch
- pointing

### 5. Test Object Interaction
In a compatible SteamVR environment, test grab and release actions using the glove.

## Troubleshooting

### ESP32 Not Detected
- Check the USB cable
- Confirm the correct COM port is selected
- Install the required serial driver
- Reflash the firmware if needed

### No Finger Movement in OpenGloves
- Confirm the ESP32 is sending serial packets
- Check sensor wiring
- Verify the packet order is correct
- Make sure the correct baud rate is selected
- Confirm OpenGloves is reading the right serial device

### Finger Motion Looks Wrong
- Re-run calibration
- Check sensor mounting alignment
- Check whether the finger order in firmware matches OpenGloves expectations
- Verify that normalized values remain in the expected range

### Vive Tracker Not Tracking
- Confirm the Base Station is powered on
- Make sure there is clear line-of-sight
- Re-pair the Vive Tracker in SteamVR
- Check that the tracker mount is stable and not shifting during movement

### Virtual Hand Pose Is Wrong
- Confirm tracker alignment on the glove
- Repeat tracker or orientation calibration in the software workflow
- Check whether the tracker is mounted in the expected orientation

### Quest 2 Display Issues
- Confirm Link mode is enabled
- Restart the Oculus PC software
- Restart SteamVR
- Check the USB connection and PC headset detection

## Validation Checklist
Use this checklist before testing the full system:

- ESP32 powers on correctly
- all five flex sensors return changing values
- serial packets are transmitted successfully
- OpenGloves receives and parses finger values
- finger calibration completes successfully
- Vive Tracker is detected in SteamVR
- Base Station tracking is stable
- virtual hand updates in real time
- gesture and interaction tests respond correctly
- Quest 2 displays the SteamVR environment

## Notes
The project architecture is designed so that the ESP32 handles only finger sensing and packet transmission, while OpenGloves handles calibration and skeletal mapping, and SteamVR handles rendering and interaction. The Quest 2 is used only as a display device, and the Vive Tracker is the primary source of hand pose tracking in the final setup.