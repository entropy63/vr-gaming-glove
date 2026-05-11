# Assembly Process

## 1. Base structure
The physical interface uses heavy-duty work gloves selected for durability and secure mounting of electronic components.

## 2. Custom sensor fabrication
Each finger sensor is made from a strip of Velostat placed between two parallel conductive copper tape strips. The structure is enclosed inside a laminating sheet for insulation and stability. The proximal end is opened so jumper wires can be soldered to the copper tape.

## 3. Circuit integration
The five sensors are connected to an ESP32 using a voltage divider circuit. Each finger has one fixed resistor and one ADC input channel. One terminal from each sensor is tied to common ground.

## 4. Mechanical assembly
Sensors are mounted on the dorsal side of the glove using adjustable straps, thermoplastic adhesive, and custom 3D-printed guide channels. Fingertip terminal clips stop the sensors from retracting or shifting during use.

## 5. Tracker mounting
An HTC Vive Tracker 3.0 is installed above the electronics stack on the dorsal side of the glove to provide 6-DoF hand tracking.

## 6. Final verification
After mounting and wiring are complete, upload the ESP32 firmware, verify serial data, calibrate in OpenGloves, and validate the virtual hand inside SteamVR.
