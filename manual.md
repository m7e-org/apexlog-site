# Apexlog User Manual

*[日本語](manual.ja) · [Home](index) · [Privacy Policy](privacy-policy)*

Apexlog records GPS + motion telemetry while you ride or drive, and lets you
replay and analyze it. This manual covers how to use the app and — importantly —
the **open NDJSON data format** so you can analyze your exported recordings
yourself.

- [Recording](#recording)
- [Your recordings (library)](#library)
- [Playback &amp; analysis](#playback)
- [Exporting](#export)
- [**Data format reference**](#data-format-reference)
- [Analyzing your data](#analyze)
- [Permissions](#permissions)
- [Free vs Pro](#free-vs-pro)

---

## Recording {#recording}

1. Mount your phone (or pocket it) and open Apexlog.
2. Tap **Record**. The status panel shows elapsed time, sample count, current
   speed and current G.
3. Tap **Stop** to finish. The run is saved automatically as an `.ndjson` file.

Each recording captures, from the moment you start (rates are the defaults —
see *Recording modes* below):

- **GPS** at ~1&nbsp;Hz — latitude, longitude, altitude, speed, heading, accuracy.
- **IMU** at ~100&nbsp;Hz — raw accelerometer, gravity-removed (user)
  acceleration, and gyroscope.

### Recording modes: Sport vs Travel

Open **Settings (gear icon) → Recording** to pick a mode:

- **Sport** (default) — the mounted, full-rate mode for track days and rides.
  You can lower the sensor rates to save battery: **IMU** Off / 25 / 50 /
  100&nbsp;Hz and **GPS** High / Balanced / Saver. The IMU drives G-force and
  lean analysis; turn it off and only the GPS track records.
- **Travel** — a low-power journey logger for trips, with the phone in your
  pocket or bag. The IMU is off and a GPS point is written **every N metres**
  rather than every second, so nothing is recorded while you stand still and a
  full day stays small (a 1-hour commute ≈ 100&nbsp;KB):
  - **On foot** — walking &amp; trains. A point every ~15&nbsp;m.
  - **Vehicle** — car &amp; motorbike. A point every ~50&nbsp;m.

  A travel recording **auto-stops after a safety limit** (6/12/24&nbsp;h,
  configurable) and posts a notification so you notice — reopen the app and
  start a new recording to continue after a hotel stop or a meal.

> **Travel mode and Pro:** travel logging is free, but keeping it running with
> the screen locked or the phone pocketed needs **background recording (Pro)**.
> Without Pro the app warns you when you start: recording will stop as soon as
> the screen locks.

### Foreground vs background

- **Free:** recording runs while Apexlog is in the foreground (on screen).
  Leaving the app or locking the screen stops capture.
- **Pro — background recording:** capture continues with the screen off or
  while another app (e.g. your navigation) is in the foreground. This is the
  intended setup for motorcycles, where the nav app owns the screen. While
  recording in the background a persistent notification (Android) / status
  indicator (iOS) is shown, as required by the OS.

---

## Your recordings (library) {#library}

The home screen lists your recordings, newest first, with file size and start
time. Tap one to open playback.

- **Free** keeps your **latest 3** recordings available; older ones are locked
  (not deleted) until you unlock Pro.
- **Pro** keeps an **unlimited** library.

Delete a recording with the trash icon. Deleting is permanent.

---

## Playback &amp; analysis {#playback}

Opening a recording shows, top to bottom:

- **Map** — your track on OpenStreetMap. Map tiles are fetched on demand and
  cached on the device.
- **Vehicle gauge** — choose from the menu:
  - **Top-down** — combined G vector seen from above, plus yaw rate.
  - **Rear view** — lean angle (and lateral G).
  - **Side view** — pitch angle (and longitudinal G).
- **G-G diagram** — the friction circle: longitudinal vs lateral G, showing how
  hard you braked, accelerated and cornered.
- **Channel chart** — speed / lean / pitch over a scrubbable timeline.

Use the timeline slider to scrub, the play button to animate, and the speed menu
(0.5×–8×) to change playback rate. Lean, pitch and heading are computed by
fusing the accelerometer, gyroscope and GPS (a complementary filter), after a
device→vehicle calibration derived from the ride itself.

**Travel recordings** (no IMU) replace the vehicle gauge with a **journey
gauge**: an activity icon that follows your speed, plus a trip odometer
(distance covered so far). *On foot* logs show Break ☕ / Walking 🚶 / Road 🚌 /
Train 🚆 / Flight ✈️; *Vehicle* logs show Stop 🚦 (standing ≤1&nbsp;min —
lights, queues) / Break ☕ (parked longer) / Local road 🚗 (&lt;60&nbsp;km/h) /
Highway 🏎 (faster). The map, speed chart and readouts work as usual.

> The on-device analysis uses a calibration that aligns the phone's arbitrary
> mounting orientation to the vehicle's axes. The **raw log stores device-frame
> sensor data** — see below if you want to reproduce the vehicle-frame math
> yourself.

---

## Exporting {#export}

**Export/share is a Pro feature.** Each recording row has an export button and
a **⋮ menu** with three ways out:

- **Export GPX / Export KML** — converts the GPS track to a standard map
  format and opens the share sheet. Your original recording is untouched; the
  export is a separate file. In **Settings → Export** you can pick which format
  the one-tap export button uses and whether to embed elevation.
- **Share raw (NDJSON)** — sends the raw `.ndjson` file exactly as described in
  the data-format reference below; nothing is stripped or transformed.

### Opening your track in Google Maps / Earth

- **Google My Maps** (view in Google Maps): on
  [mymaps.google.com](https://mymaps.google.com) create a map → **Import** →
  choose the exported **KML** (or GPX). The route draws on the map and syncs to
  the Google Maps app under *Saved → Maps*.
- **Google Earth**: [earth.google.com](https://earth.google.com) → Projects →
  **Import KML file** — or just open the KML file on a device with Earth
  installed.
- **GPX** also imports into Strava, Garmin Connect, komoot, Ride with GPS and
  most other GPS tools (as an activity or route, depending on the app).

On a Mac/PC you can also copy the files directly off the device over USB (they
live in the app's Documents directory).

---

## Data format reference {#data-format-reference}

Recordings are **UTF-8 NDJSON** (newline-delimited JSON): **one JSON object per
line**, one sample per line. The format is open and trivial to parse in any
language.

### File

```
apexlog-YYYYMMDD-HHMMSS.ndjson
```

Named from the local wall-clock start time, e.g.
`apexlog-20260615-181241.ndjson`.

- **Line 1 is always the `meta` line.**
- Every following line is one **`gps`** or **`imu`** sample, in roughly
  time-generated order.

### `meta` line (first line only)

```json
{"type":"meta","app":"apexlog","ver":"0.2.1","startedAt":"2026-06-15T18:12:41.463902+09:00","device":"iPhone16,2","hz":{"imu":100,"gps":1},"mode":"sport"}
```

| Key | Meaning |
|---|---|
| `type` | Constant `"meta"` |
| `app` | Constant `"apexlog"` |
| `ver` | Log-format / app version (e.g. `"0.2.1"`) |
| `startedAt` | Recording start wall-clock, ISO 8601 with local UTC offset |
| `device` | Device model (e.g. `"iPhone16,2"`, `"Pixel 8"`; `"unknown"` if unavailable) |
| `hz` | **Requested** sampling rates (configured, not measured). `imu` is `0` when the IMU was disabled (travel mode / IMU Off) — such logs contain no `imu` lines. `gps` is the nominal rate of the chosen tier; `0` means sub-Hz (distance-driven travel presets) |
| `mode` | *(0.2.1+, optional)* `"sport"` or `"travel"`. Absent in older logs |
| `preset` | *(0.2.1+, optional)* Travel preset: `"onFoot"` or `"vehicle"` |

### Sample lines

Every sample line carries:

| Key | Meaning | Unit |
|---|---|---|
| `t` | Seconds since recording start (monotonic clock) | s |
| `s` | Stream: `"gps"` or `"imu"` | — |

**`gps` line:**

```json
{"t":1.000,"s":"gps","lat":35.681236,"lon":139.767125,"alt":3.2,"spd":12.5,"hdg":270.1,"acc":4.0}
```

| Key | Meaning | Unit |
|---|---|---|
| `lat` / `lon` | Latitude / longitude | deg |
| `alt` | Altitude | m |
| `spd` | Speed | m/s |
| `hdg` | Heading (course over ground); **negative = unavailable** (e.g. at standstill) | deg |
| `acc` | Horizontal position accuracy | m |

**`imu` lines.** The three IMU sensors arrive as **independent stream events at
different instants**, so each is written on **its own line containing only that
sensor's axes** (other fields omitted). A line is one of:

```json
{"t":0.010,"s":"imu","ax":0.12,"ay":-0.03,"az":9.79}
{"t":0.011,"s":"imu","lax":0.05,"lay":-0.01,"laz":0.02}
{"t":0.010,"s":"imu","gx":0.001,"gy":0.000,"gz":-0.002}
```

| Keys | Meaning | Unit |
|---|---|---|
| `ax` / `ay` / `az` | Raw accelerometer (**gravity included**) | m/s² |
| `lax` / `lay` / `laz` | User acceleration (**gravity removed**) | m/s² |
| `gx` / `gy` / `gz` | Gyroscope (angular velocity) | rad/s |

Notes:

- Axes are in the **device frame** (the phone's own X/Y/Z), exactly as the OS
  sensor APIs report them — *not* aligned to the vehicle. Mount orientation is
  arbitrary; align to the vehicle yourself if needed (see below).
- Numeric precision in the file: `lat`/`lon` 7 dp; `alt`/`spd`/`hdg`/`acc` 2 dp;
  accel/gyro 5 dp; `t` 3 dp. Non-finite values are written as `0`.
- Streams are **not** time-aligned. To get accel + gyro at the same instant,
  interpolate each stream independently over `t`.
- `t` comes from a monotonic stopwatch (immune to wall-clock changes); map back
  to absolute time with `meta.startedAt + t`.

---

## Analyzing your data {#analyze}

A minimal Python example — load a recording, separate the streams, and compute
total G and speed in km/h:

```python
import json, math

meta = None
gps, accel, uaccel, gyro = [], [], [], []

with open("apexlog-20260615-181241.ndjson") as f:
    for line in f:
        o = json.loads(line)
        if o.get("type") == "meta":
            meta = o; continue
        t = o["t"]
        if o["s"] == "gps":
            gps.append(o)
        elif "ax" in o:
            accel.append(o)
        elif "lax" in o:
            uaccel.append(o)
        elif "gx" in o:
            gyro.append(o)

# Total acceleration magnitude in G (gravity included)
G0 = 9.80665
for a in accel:
    a["g"] = math.sqrt(a["ax"]**2 + a["ay"]**2 + a["az"]**2) / G0

# GPS speed in km/h
for p in gps:
    p["kmh"] = p["spd"] * 3.6

print(meta["device"], len(gps), "GPS fixes,", len(accel), "accel samples")
```

Handy conversions:

- **Speed:** `km/h = spd × 3.6`, `mph = spd × 2.23694`.
- **Total G (with gravity):** `sqrt(ax² + ay² + az²) / 9.80665`.
- **Linear G (motion only):** use `lax/lay/laz` instead of `ax/ay/az`.
- **Gyro to deg/s:** `deg/s = rad/s × 180 / π`.
- **Vehicle frame:** estimate "up" from the average of the raw accelerometer
  (specific force points up at rest), find "forward" by correlating
  longitudinal GPS acceleration with the horizontal IMU, and take
  "lateral = up × forward". This is what the in-app calibration does before
  computing lean/pitch.

Pandas tip: read with `pandas.read_json(path, lines=True)`, drop the first
(`meta`) row, then split by the presence of columns (`ax`, `lax`, `gx`, `lat`).

---

## Permissions {#permissions}

- **Location (while in use; background is Pro):** to record your GPS track.
  Background access only continues a recording you started.
- **Motion &amp; fitness / sensors:** accelerometer and gyroscope.
- **Notifications (Android):** the ongoing "recording" notification the OS
  requires for background capture.
- **Internet:** to download map tiles during playback and to process purchases.

You can revoke any permission in system settings; features that depend on it
stop until you grant it again.

---

## Free vs Pro {#free-vs-pro}

| | Free | Pro (one-time) |
|---|---|---|
| Recording (Sport &amp; Travel modes) | Foreground | **＋ Background** |
| Saved logs | Latest 3 | **＋ Unlimited** |
| Map / G-G / attitude / journey playback | ✓ Full | ✓ Full |
| Share raw &amp; GPX/KML export | — | **✓** |

**Pro is a one-time purchase — ¥1,000 / US$6.99. No subscription.** Restore a
previous purchase from the paywall (tap **Pro**, then **Restore**).
