# Fall Detection System Runbook

This guide explains how to run the deploy system, where the current laptop/server IP is configured, and what changed in the latest update.

Current server IP: `10.195.218.150`

## System Pieces

- ESP32 wearable sends sensor batches to the Node server over CoAP UDP on port `5683`.
- Node server receives sensor data, serves REST APIs on port `3000`, broadcasts realtime data with Socket.IO, and calls the Python ML service.
- Python ML service runs locally on the laptop at `http://127.0.0.1:5000/predict`.
- Web dashboard reads realtime accel, gyro, magnitude, BPM, inference timing, and fall events from the Node server.
- Mobile app reads realtime BPM, sends owner phone GPS to the server, and lets relatives fetch the wearer phone GPS.

## Quick Start

Run the full RF deploy stack:

```powershell
cd E:\PBL5\source\Fall-Detection-Smart-Wearable-Device-
.\start-all.bat
```

`start-all.bat` now calls `start-all-deploy-rf.bat`, which starts:

- RF Python inference service: `python_ml\start-ml-rf.bat`
- Deploy Node server: `server\npm run start:deploy`
- Web dashboard: `client\index_v2.html`

Use this instead if you specifically want the LSTM Python service:

```powershell
cd E:\PBL5\source\Fall-Detection-Smart-Wearable-Device-
.\start-all-deploy-lstm.bat
```

## Manual Start

Terminal 1, RF ML service:

```powershell
cd E:\PBL5\source\Fall-Detection-Smart-Wearable-Device-\python_ml
.\start-ml-rf.bat
```

Terminal 2, deploy server:

```powershell
cd E:\PBL5\source\Fall-Detection-Smart-Wearable-Device-\server
$env:SERVER_IP = "10.195.218.150"
$env:PREFERRED_IP_PREFIX = "10.195.218"
npm run start:deploy
```

The server should print URLs similar to:

```text
Local:   http://localhost:3000
Network: http://10.195.218.150:3000
CoAP:    coap://10.195.218.150:5683
ML:      http://127.0.0.1:5000/predict
```

## If Laptop IP Changes

Find the new laptop IPv4 address on the same Wi-Fi/LAN as the ESP32 and phone:

```powershell
ipconfig
```

Then update these places.

Wearable/server project:

- `start-all-deploy-rf.bat`
  - `SERVER_IP=NEW_IP`
  - `PREFERRED_IP_PREFIX=first.three.octets`
- `start-all-deploy-lstm.bat`
  - same values as above
- `client\index_v2.html`
  - `DEFAULT_SERVER_URL = 'http://NEW_IP:3000'`
- `esp32_code\fall_detection_arduino_deploy.ino`
  - `serverHost = "NEW_IP"`
  - Re-upload the firmware to the ESP32 after changing this.

Mobile app project, `E:\PBL5\source\pbl5-fall-detection-app`:

- `lib\app.dart`
  - default server URL fallback
- `lib\main.dart`
  - notification bootstrap fallback
- `lib\screens\settings\settings_screen.dart`
  - hint text only, optional but useful

Important saved-settings note:

- The dashboard stores the server URL in browser local storage under `fall_dashboard_server_url`. If it still connects to an old IP, edit the server URL field in the dashboard or clear browser local storage.
- The mobile app stores the server URL in SharedPreferences under `server_url`. If it still connects to an old IP, update/reset the server URL in the mobile settings screen.

## Network Checklist

- Laptop, ESP32, and phone should be on the same Wi-Fi/LAN.
- Windows firewall must allow Node on TCP `3000`.
- Windows firewall must allow CoAP UDP `5683`.
- Python ML service stays local to the laptop, so `127.0.0.1:5000` is correct for the Node server.
- If you change server `PORT`, every HTTP URL must use the new port.
- If you change `COAP_PORT`, ESP32 firmware and server config must match.

## Latest Changes Made

Server, normal and deploy:

- Sensor data can keep arriving continuously over CoAP/HTTP.
- Realtime sensor data is still emitted to dashboard even when ML is skipped.
- ML inference now runs only when `fsm_state === 2`.
- When FSM is not `2`, server returns/emits:
  - `ml_processed: false`
  - `ml_skip_reason: "fsm_not_impact"`
- Impact windows wait until the required ML sample window is full before calling Python ML.
- Server emits `heartRateUpdated` so mobile can show realtime BPM.
- Timing metrics were added:
  - `batch_received_at`
  - `inference_started_at`
  - `inference_finished_at`
  - `inference_duration_ms`
  - `fall_detected_at`
  - `fall_detection_latency_ms`
- Backend logs now include:
  - `[/http/sensor-batch] inference_timing`
  - `[/coap/sensor-batch] inference_timing`
  - `[fall-detection] timing`
- Phone GPS endpoints were added:
  - `POST /api/wearer-location`
  - `GET /api/wearer-location`

Web dashboard:

- Default server URL is now `http://10.195.218.150:3000`.
- Realtime line charts show accel, gyro, magnitude, and BPM.
- BPM is also shown as a realtime value.
- ML status, inference time, and fall detection latency are shown on the dashboard.

Mobile app:

- Default server URL is now `http://10.195.218.150:3000`.
- Mobile shows realtime BPM.
- Owner role sends phone GPS to the server.
- Relative role fetches the wearer phone GPS from the server.
- Relative home screen uses the wearer/owner phone location instead of relative phone GPS.

Deploy startup and firmware:

- `start-all.bat` now calls the existing RF deploy launcher.
- `start-all-deploy-rf.bat` and `start-all-deploy-lstm.bat` set `SERVER_IP=10.195.218.150`.
- ESP32 deploy firmware now targets `10.195.218.150`.

## Expected Behavior

- Dashboard should show realtime accel/gyro lines and realtime BPM while the ESP32 is sending data.
- Mobile should show realtime BPM only.
- Server should not call ML until `fsm_state` is `2`.
- After `fsm_state` is `2`, server waits for enough samples, calls Python ML, logs inference timing, and sends fall events if ML qualifies.
- Owner phone location is stored on the server; relatives can fetch that wearer location.

## Troubleshooting

No dashboard connection:

- Check the server printed `Network: http://10.195.218.150:3000`.
- Check the dashboard server URL field.
- Clear dashboard local storage if it keeps using an old IP.
- Check Windows firewall for TCP `3000`.

No ESP32 data:

- Confirm ESP32 firmware `serverHost` matches the laptop IP.
- Re-upload firmware after changing `serverHost`.
- Check Windows firewall for UDP `5683`.
- Check server log for `CoAP UDP deploy server listening on port 5683`.

No ML inference:

- Confirm Python ML service is running.
- Confirm ESP32 sends `fsm_state: 2` during impact/fall state.
- If server says `waiting_for_full_window`, it is buffering until enough samples arrive.

No mobile GPS for relative:

- Owner phone must run the app as owner and grant location permission.
- Owner phone must be connected to the server.
- Relative phone fetches from `GET /api/wearer-location`.

## Verification Commands

Server syntax:

```powershell
cd E:\PBL5\source\Fall-Detection-Smart-Wearable-Device-\server
node --check server.js
node --check server_deploy.js
```

Mobile checks:

```powershell
cd E:\PBL5\source\pbl5-fall-detection-app
dart format lib\app.dart lib\main.dart lib\screens\settings\settings_screen.dart
flutter analyze
```

