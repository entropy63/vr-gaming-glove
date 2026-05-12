# Test Results Summary

This document summarizes the final testing outcomes for the VR Gaming Glove prototype. It covers unit, integration, system, and user acceptance testing based on the completed Chapter 5 implementation and testing report.

## Overview

The final test cycle included 21 test cases across four testing levels:

- Unit Testing: 6
- Integration Testing: 5
- System Testing: 7
- User Acceptance Testing: 3

All 21 test cases passed successfully.

## Final Status

| Test Level | Total | Passed | Failed | Pending |
|---|---:|---:|---:|---:|
| Unit Testing | 6 | 6 | 0 | 0 |
| Integration Testing | 5 | 5 | 0 | 0 |
| System Testing | 7 | 7 | 0 | 0 |
| User Acceptance Testing | 3 | 3 | 0 | 0 |
| **Total** | **21** | **21** | **0** | **0** |

## Key Measured Results

| Metric | Requirement / Target | Achieved Result | Status |
|---|---|---|---|
| Active finger channels | 5 | 5 | Pass |
| SteamVR device registration | Successful | Successful | Pass |
| Skeletal hand animation | Real-time | Real-time | Pass |
| Battery endurance | 8 hours minimum | 10 hours 23 minutes | Pass |
| End-to-end latency | Less than 50 ms | 24-31 ms, average 27.2 ms | Pass |
| Connection stability | No dropouts | No dropouts observed | Pass |
| Finger normalization range | 0.0-1.0 | 0.0-1.0 | Pass |
| Calibration time | Less than 30 seconds | Average 21.7 seconds | Pass |
| Comfort rating | At least 3/5 | 3.7 at 30 min, 3.3 at 60 min | Pass |
| Interaction satisfaction | At least 3.5/5 | 3.86/5 average | Pass |

## Unit Test Highlights

The firmware and sensor pipeline passed all unit-level validation checks.

- Thumb ADC reading stayed within expected range: 3350 when flat and 1814 when bent.
- Each individual finger changed only its own channel, with adjacent channels changing by at most 12 ADC counts.
- Exponential smoothing reduced jitter from a standard deviation of 18.4 counts to 4.2 counts, a 77% reduction.
- Normalization correctly mapped flat fingers to 0.00, bent fingers to 1.00, and mid-bend values to 0.47-0.53.
- All 20 sampled serial packets contained exactly 5 comma-separated values and no malformed packets.
- All ADC input voltages remained within safe ESP32 limits, with flat values between 2.64 V and 2.70 V and bent values between 1.32 V and 1.57 V.

## Integration Test Highlights

Subsystem communication between ESP32, OpenGloves, SteamVR, and Vive Tracker worked correctly.

- OpenGloves detected the ESP32 automatically and displayed live finger values in real time.
- All five physical fingers mapped to the correct virtual fingers with no cross-mapping errors.
- Vive Tracker pose tracking showed accurate translation and rotation with no observable drift during the evaluation period.
- Finger articulation and tracker pose were fused correctly with no visible desynchronization.
- After a USB disconnect, OpenGloves reconnected automatically within 6 seconds without requiring a restart.

## System Test Highlights

The full glove pipeline met the major functional and non-functional requirements.

- SteamVR recognized the glove as `OpenGloves Controller Left Hand` with active device status.
- Measured end-to-end latency across five trials was 24 ms, 27 ms, 31 ms, 28 ms, and 26 ms.
- Gesture testing achieved 39/40 correct detections on first attempt and 40/40 on re-test, with zero false positives.
- Virtual object grab and release worked correctly in all five interaction trials.
- The system ran continuously for 10 hours 23 minutes before battery voltage dropped below the ESP32 operating threshold.
- A 30-minute VR session completed without crashes, disconnects, or driver failures.

## User Acceptance Results

User-facing tests showed the glove was usable and practical for short VR sessions.

- Three new users completed calibration in 18 s, 22 s, and 25 s, for an average of 21.7 seconds.
- Comfort ratings averaged 3.7/5 at 30 minutes and 3.3/5 at 60 minutes.
- Interaction accuracy ratings averaged 3.86/5 across grab, place, point, pinch, and wave tasks.
- Pinch received the lowest average score at 3.3/5, indicating that gesture thresholds may still need fine-tuning.

## Observations

All tests passed, but a few minor observations were recorded for future improvement.

- One pinch gesture was missed on first attempt due to slightly slow finger closure.
- One brief tracker occlusion of about 1 second occurred when the hand moved behind the user's body.
- One subject reported mild wrist stiffness after about 55 minutes because of Vive Tracker weight.

## Conclusion

The final prototype passed all defined test cases and satisfied the projects primary performance, usability, and compatibility goals. The results confirm that the glove is ready for final demonstration as a low-cost SteamVR-compatible VR hand input prototype.