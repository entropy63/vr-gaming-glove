# Assembly Process

This document describes the physical assembly process of the VR Gaming Glove prototype. The glove is built as a wearable embedded system that combines five flex sensors, an ESP32 microcontroller, a Vive Tracker 3.0 mount, and supporting wiring for real-time hand interaction in SteamVR.

## Assembly Goals
The assembly process is designed to achieve the following goals:

- mount one flex sensor on each finger
- route all sensor wiring safely to the ESP32
- keep the glove lightweight and wearable
- attach the Vive Tracker 3.0 securely to the back of the hand
- support stable calibration and repeatable testing
- maintain compatibility with OpenGloves and SteamVR workflows

## Main Components
The physical glove assembly includes:

- glove base
- five flex sensors
- ESP32 development board
- wiring and connectors
- resistors for analog input support
- Micro-USB cable
- adhesives or mounting tape
- 3D-printed supports or holders if available
- Vive Tracker 3.0
- tracker mounting plate or bracket

## Assembly Overview
The glove is assembled in four main stages:

1. Fabricate or prepare the flex sensors
2. Mount the sensors onto the glove
3. Connect the sensors to the ESP32
4. Attach the Vive Tracker and finalize cable routing

## Step 1: Prepare the Glove Base
Choose a glove that is lightweight, flexible, and comfortable enough for short VR sessions. The glove should allow natural hand movement and provide enough surface area to mount sensors and route wires without restricting finger motion.

Before mounting any electronics:

- confirm the glove fits the intended user
- identify the top side of each finger for sensor placement
- mark a mounting position for the ESP32
- mark a stable mounting location on the back of the hand for the Vive Tracker

## Step 2: Prepare the Flex Sensors
Each finger uses one flex sensor. The report design supports low-cost custom sensor construction using materials such as Velostat, copper tape, and insulating layers.

If custom sensors are used:

1. Cut the sensor materials to finger length
2. Build the conductive layers carefully
3. Add insulation to avoid short circuits
4. Attach wire leads securely to each sensor end
5. Label each sensor by finger name

Recommended sensor labels:

- Thumb
- Index
- Middle
- Ring
- Pinky

This labeling helps maintain a fixed wiring order for firmware and OpenGloves packet formatting.

## Step 3: Mount the Sensors on the Fingers
Attach one sensor to each finger in the same orientation so that finger bending produces a measurable change in resistance. The sensor should sit along the finger in a way that captures curl motion but does not block full hand movement.

Mounting guidelines:

- place the sensor where finger bending is strongest
- keep the sensor centered and aligned
- avoid sharp folds in the sensor body
- leave enough slack near joints
- secure the sensor firmly so it does not shift during use

Possible mounting methods:

- fabric tape
- adhesive strips
- stitched guides
- 3D-printed holders
- textile sleeves

## Step 4: Route the Sensor Wires
After mounting all five sensors, route the wires from the fingers toward the wrist or back of the hand, depending on the ESP32 location. Keep the layout organized to reduce strain, tangling, and accidental wire pull.

Wire routing guidelines:

- keep wires close to the glove surface
- avoid crossing over finger joints where possible
- bundle wires neatly by direction
- add strain relief near sensor connections
- secure long wire segments with tape or clips

A clean wire layout improves durability and reduces movement noise during testing.

## Step 5: Mount the ESP32
Attach the ESP32 to a stable position on the glove, typically near the wrist or back of the hand, depending on space and comfort. The board must remain accessible for USB connection, firmware upload, and troubleshooting.

The mounting location should:

- avoid pressing into the user’s skin
- keep the USB port reachable
- minimize cable bending
- reduce interference with hand motion
- support safe wire routing from all five sensors

Possible mounting methods:

- Velcro strap
- zip ties
- adhesive-backed platform
- 3D-printed enclosure
- stitched pocket

## Step 6: Connect the Sensors to the ESP32
Wire each flex sensor into the analog input interface expected by the ESP32 firmware. The report design requires the ESP32 to read five analog channels and provide structured finger values to the PC.

During connection:

- connect each sensor to the intended analog input pin
- verify power and ground paths
- add required resistors for stable analog reading
- keep the finger order consistent with firmware expectations
- double-check continuity before powering the board

Expected logical order:

1. Thumb
2. Index
3. Middle
4. Ring
5. Pinky

Maintaining this order is important because OpenGloves expects a fixed packet layout for finger values.

## Step 7: Secure the Wiring
Once the ESP32 connections are complete, secure all loose wires to prevent movement during use. Do not leave unsupported wires hanging from the glove.

Recommended practices:

- tape wires along the glove surface
- group sensor lines into a small harness
- keep the USB cable path clear
- protect solder joints or exposed connections
- make sure nothing sharp or rigid touches the user’s skin

## Step 8: Attach the Vive Tracker Mount
The Vive Tracker 3.0 must be mounted on the back of the hand using a stable attachment method. The tracker should remain rigidly aligned with the glove so that hand pose in SteamVR stays consistent during calibration and use.

Mounting requirements:

- place the tracker on the back of the hand
- keep it centered and stable
- avoid tilt or wobble during movement
- do not block the tracking sensors
- preserve comfort and hand mobility

Possible mounting options:

- official tracker mounting plate
- custom 3D-printed bracket
- rigid strap mount
- reinforced fabric platform

## Step 9: Check Physical Safety
Before powering the system, inspect the glove for comfort and safety.

Confirm that:

- no exposed conductors touch the user
- no component exceeds safe low-voltage operation
- the glove has no sharp edges
- the tracker and ESP32 are secured firmly
- finger movement is not restricted
- the USB cable does not pull on the glove during use

## Step 10: Perform Initial Power Test
Connect the ESP32 to the PC using Micro-USB and verify that the board powers on correctly. At this stage, the goal is only to confirm electrical readiness and basic sensor behavior.

Check for the following:

- ESP32 powers on normally
- no wire heats up
- no sensor disconnects during finger motion
- serial output changes as fingers bend
- the board remains mechanically stable when worn

## Mechanical Design Notes
The report supports the use of 3D-printed enclosures, cable guides, and tracker mounts as part of the assembly process. These parts are optional but useful for improving mechanical stability, cable management, and repeatability across test sessions.

3D-printed parts may be used for:

- ESP32 housing
- sensor holders
- wire guides
- tracker bracket
- protective covers

## Integration Notes
The physical assembly directly affects the software calibration process. If sensors shift, wiring loosens, or the Vive Tracker mount rotates during use, OpenGloves calibration and SteamVR hand alignment may become unstable.

For that reason, the assembly should prioritize:

- consistent sensor placement
- fixed finger order
- stable tracker orientation
- comfortable wearability
- repeatable setup between sessions

## Final Checklist
Before moving to firmware upload and software calibration, confirm the following:

- five sensors are mounted and labeled
- sensor wires are routed cleanly
- ESP32 is mounted securely
- all analog connections are complete
- USB access is available
- Vive Tracker is mounted firmly
- glove remains comfortable to wear
- no unsafe or loose parts remain

## Notes
The assembly described here reflects the academic prototype design documented in the project report. The mechanical structure is intentionally modular so that sensors, tracker mounts, wiring layouts, and 3D-printed parts can be improved or replaced in future versions without redesigning the full system.