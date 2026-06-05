# System Flow

Flow tổng thể của hệ thống IoT:

```text
MPU6050 + MAX30102
        |
        v
ESP32-C3 trên thiết bị đeo
        |
        +--> FSM + buzzer cảnh báo tại chỗ
        |
        +--> CoAP/HTTP sensor batch
                |
                v
        Node.js server
                |
                +--> Flask ML service: Gradient Boosting
                |
                +--> Website dashboard realtime
                |
                +--> App Flutter cảnh báo và phản hồi
```

Sơ đồ này cần được đưa trực tiếp vào Chương 2 để thể hiện rõ tính IoT của hệ thống: cảm biến, vi điều khiển, truyền thông mạng, xử lý server và giao diện người dùng.

Ảnh placeholder: `../../images-report/system-iot-block-diagram.png`.
