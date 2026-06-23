---
title: Apexlog — Foreground service (location) usage
---

# Foreground service (location) — usage demo

Apexlog uses the `FOREGROUND_SERVICE_LOCATION` permission to keep a ride/drive
recording running when the screen is off or a navigation app is in the
foreground. While recording, the app logs the GPS track together with high‑rate
motion sensor data (accelerometer / gyroscope) to an on‑device file.

The clip below shows a recording in progress, then the persistent
foreground‑service notification ("apexlog is recording — Logging GPS and motion
in the background"), then returning to the app with the recording still active.
The recorded location stays on the device and is never transmitted.

<video src="assets/apexlog_fgs_demo.mp4" controls playsinline width="320"></video>

[Direct video link](assets/apexlog_fgs_demo.mp4)
