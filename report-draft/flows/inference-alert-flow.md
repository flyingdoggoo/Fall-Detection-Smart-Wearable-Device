# Inference And Alert Flow

Flow suy luận và cảnh báo:

```text
ESP32-C3 sends sensor batch
        |
        v
Node.js server validates payload
        |
        v
Server calls Flask /predict
        |
        v
Gradient Boosting returns fall_detected + confidence
        |
        +--> Not fall: continue monitoring
        |
        +--> Fall:
                |
                v
        Create fall event
                |
                +--> Emit realtime to website
                |
                +--> Send alert to Flutter app
                |
                +--> User/relative responds
                |
                v
        Server updates event status
```

Ảnh hiện có: `Fall-alert-1.png`, `Fall-alert-2.png`, `relative-received-alert.png`, `relative-safe.png`, `web-monitoring.png`.

Ảnh cần bổ sung: `coap-http-communication-flow.png`, `software-architecture.png`.
