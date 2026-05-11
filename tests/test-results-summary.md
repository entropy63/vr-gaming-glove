# Test Results Summary

| Level | Total | Passed | Failed |
|---|---|---|---|
| Unit Testing | 6 | 6 | 0 |
| Integration Testing | 5 | 5 | 0 |
| System Testing | 7 | 7 | 0 |
| User Acceptance Testing | 3 | 3 | 0 |
| **Total** | **21** | **21** | **0** |

## Key Performance Results

| Metric | Target | Achieved | Result |
|---|---|---|---|
| Active finger channels | 5 | 5 | Pass |
| SteamVR registration | Successful | Successful | Pass |
| Skeletal hand animation | Real-time | Real-time | Pass |
| Battery endurance | 8 h | 10 h 23 min | Pass |
| End-to-end latency | < 50 ms | 24–32 ms | Pass |
| Connection stability | No dropouts | No dropouts | Pass |
| Normalized bend range | 0.0–1.0 | 0.0–1.0 | Pass |

## Latency Breakdown

| Pipeline Stage | Latency |
|---|---|
| ADC sampling (5 channels) | 0.5–1 ms |
| Exponential smoothing filter | 0.5 ms |
| Packet build + serial TX 115200 baud | 8–12 ms |
| OpenGloves parse + finger mapping | 2–4 ms |
| Vive Tracker pose acquisition | 1–2 ms |
| Pose fusion (finger + tracker) | 1–2 ms |
| SteamVR skeletal render (90 Hz) | 11 ms/frame |
| **Total** | **24–32 ms** |

## Battery Endurance

| Battery | Avg Current | Calculated | Observed | Result |
|---|---|---|---|---|
| 1000 mAh | 95 mA | 10.5 h | 10 h 23 min | Pass |
