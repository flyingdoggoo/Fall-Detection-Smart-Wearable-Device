# Hardware And Power Flow

Flow phần cứng và nguồn:

```text
Pin LiPo 3.7V 2000mAh
        |
        +--> TP4056: sạc pin
        |
        +--> MT3608: tăng áp/ổn định nguồn
                |
                +--> ESP32-C3
                +--> MPU6050
                +--> MAX30102
                +--> Buzzer
```

Khối cảm biến dùng I2C để truyền tín hiệu về ESP32-C3. Buzzer được điều khiển bằng GPIO/PWM để cảnh báo tại chỗ khi FSM xác nhận nguy cơ té ngã.

Kết nối chân theo firmware:

| Tín hiệu | Chân ESP32-C3 | Kết nối |
|---|---|---|
| I2C SDA | GPIO4 | SDA của MPU6050 và MAX30102 |
| I2C SCL | GPIO5 | SCL của MPU6050 và MAX30102 |
| Buzzer | GPIO18 | Chân điều khiển buzzer bằng PWM |
| Button | GPIO9 | Nút nhấn điều khiển start/stop, dùng INPUT_PULLUP |
| Nguồn cảm biến | 3V3 | VCC của MPU6050 và MAX30102 |
| Mass chung | GND | GND của cảm biến, nguồn và ESP32-C3 |

Ảnh hiện có: `mpu6050.jpg`, `max30102.jpg`, `tp4056.jpg`, `lipo3.7v.jpg`, `mt3608.jpg`, `buzzer.jpg`.

Ảnh cần bổ sung: `hardware-schematic.png`, `sensor-wiring-diagram.png`, `power-circuit-diagram.png`.
