# Privacy Policy — Apexlog

**Effective date:** 2026-06-16
**Last updated:** 2026-06-16

Apexlog ("the app", "we", "us") is a telemetry logger that records GPS and
motion-sensor data while you ride or drive, for you to review and analyze later.

This policy explains what the app processes and what leaves your device. The
short version: **your recordings stay on your device. We do not operate any
server, we have no account system, and we do not collect, transmit, sell, or
share your recordings.**

---

## 1. Who is responsible

The app is published by SANEI DENKI, Y.K. (有限会社山栄電機), 5-1-50
Kamitsuchidana-minami, Ayase, Kanagawa, Japan. For any privacy question, contact
support@sanei.m7e.org.

## 2. What the app processes, and where it is stored

When you make a recording, the app reads the following from your device and
writes it to a file (NDJSON) in the app's private storage **on your device**:

| Data | Examples | Purpose |
|---|---|---|
| Location | latitude, longitude, altitude, speed, heading, accuracy | The GPS track of your ride/drive |
| Motion sensors | accelerometer, gyroscope (G-force, rotation) | The motion/attitude of your ride/drive |
| Device model | e.g. "Pixel 9 Pro", "iPhone16,2" | Stored once in the log header for context |

This data is stored **locally only**. It is never uploaded to us — we have no
servers to receive it. It remains on your device until you delete the recording
or uninstall the app.

We also store, locally, whether you have purchased the "Pro" upgrade (a single
on/off flag in the app's preferences).

## 3. What we do NOT do

- We do **not** have user accounts or require sign-in.
- We do **not** upload, back up, or sync your recordings to any cloud service.
- We do **not** use analytics, advertising, tracking, or crash-reporting SDKs.
- We do **not** sell or share your personal data.
- We do **not** receive your location or motion data at all.

## 4. Third-party services

The app contacts third parties only in these limited cases:

- **Map tiles (OpenStreetMap).** When you open a recording for playback, the app
  downloads map imagery for the area of your track from the OpenStreetMap tile
  servers and caches the tiles on your device. These requests necessarily reveal
  the geographic area you are viewing to the tile provider. See the OpenStreetMap
  Foundation privacy policy: https://wiki.osmfoundation.org/wiki/Privacy_Policy
- **In-app purchases (Apple / Google).** The optional "Pro" upgrade is processed
  by the App Store or Google Play. Payment and account details are handled by
  Apple/Google under their own privacy policies; the app only receives a
  confirmation that the purchase is active. We never see your payment details.

We do not embed any other third-party networking, advertising, or analytics
components.

## 5. Sharing and export (only when you ask)

The app can share or export a recording file when **you** choose to. When you do,
the file leaves your device through your operating system's standard share
sheet, to the destination you pick (email, Files, AirDrop, another app, etc.).
That destination's handling of the file is outside our control. We are not
involved in, and do not receive, these transfers. (Share/export is a Pro feature.)

## 6. Permissions the app requests, and why

- **Location (while in use, and — Pro only — in the background):** to record your
  GPS track. Background access lets recording continue when another app (such as
  navigation) is on screen or the screen is off, e.g. on a motorcycle. Background
  location is used **only** to continue an active recording you started.
- **Motion & fitness / sensors:** to record accelerometer and gyroscope data.
- **Notifications (Android):** to show the ongoing "recording" notification that
  the system requires while recording in the background.
- **Internet:** to download map tiles during playback and to process purchases.

You can revoke any permission at any time in your device settings; recording
features that depend on it will stop working until it is granted again.

## 7. Your choices and controls

- **Delete recordings** individually inside the app at any time.
- **Uninstall** the app to remove all recordings and stored data from your device.
- **Revoke permissions** in your device settings.
- Because we never receive your data, there is nothing held by us to request,
  correct, or delete.

## 8. Data retention

Recordings and the Pro flag are kept on your device until you delete them or
uninstall the app. We retain nothing, because we receive nothing.

## 9. Children

The app is not directed to children and does not knowingly process data from
children. It is a general-audience utility for vehicle telemetry.

## 10. Security

Recordings are stored in the app's private, sandboxed storage provided by the
operating system. As the data never leaves your device except when you
explicitly share it, the primary safeguard is your device's own security
(passcode/biometrics, encryption).

## 11. International users

Your recordings are processed and stored locally on your device wherever you
are. We do not transfer your recordings across borders because we do not receive
them. Map-tile and purchase requests are handled by the respective providers
under their own terms.

## 12. Changes to this policy

If the app's data practices change, we will update this policy and its "Last
updated" date, and (where appropriate) note the change in the app or store
listing.

## 13. Contact

Questions about this policy or the app's privacy practices:
SANEI DENKI, Y.K. — support@sanei.m7e.org

_Governing context: Japan._
