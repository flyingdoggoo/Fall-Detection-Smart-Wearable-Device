# Mermaid Flow Sources

Các file `.mmd` trong thư mục này là nguồn để render ra ảnh PNG đang được tham chiếu trong báo cáo chính.

| File Mermaid | Ảnh cần xuất ra | Mục báo cáo |
|---|---|---|
| `system-iot-block-diagram.mmd` | `images-report/system-iot-block-diagram.png` | `2.1.1` |
| `sensor-wiring-diagram.mmd` | `images-report/sensor-wiring-diagram.png` | `2.1.2` |
| `power-circuit-diagram.mmd` | `images-report/power-circuit-diagram.png` | `2.1.3` |
| `hardware-schematic.mmd` | `images-report/hardware-schematic.png` | `2.1.4` |
| `coap-http-communication-flow.mmd` | `images-report/coap-http-communication-flow.png` | `2.1.6` |
| `data-collection-scenarios.mmd` | `images-report/data-collection-scenarios.png` | `2.2.2` |
| `software-architecture.mmd` | `images-report/software-architecture.png` | `2.3.1` |

Ví dụ render bằng Mermaid CLI:

```powershell
mmdc -i "report-draft/mermaid/system-iot-block-diagram.mmd" -o "images-report/system-iot-block-diagram.png"
```

Các ảnh `gb-confusion-matrix.png`, `gb-roc-curve.png`, `gb-pr-curve.png` và `gb-feature-importance.png` không được tạo bằng Mermaid vì đây là biểu đồ kết quả mô hình, nên cần xuất trực tiếp từ notebook hoặc script đánh giá Gradient Boosting.
