# Testing Flow

Flow kiểm thử được chia theo phân hệ:

```text
Hardware test
        |
        +--> MPU6050, MAX30102, nguồn, buzzer

Communication test
        |
        +--> CoAP sensor batch
        +--> HTTP API
        +--> Socket.IO realtime dashboard

AI test
        |
        +--> LOSO metrics
        +--> Gradient Boosting confusion matrix
        +--> ROC/PR curve

App/Web test
        |
        +--> Website dashboard
        +--> Flutter alert
        +--> Safe/help response
```

Curve/confusion matrix chỉ cần trình bày cho mô hình triển khai Gradient Boosting.

Placeholder kết quả model: `gb-confusion-matrix.png`, `gb-roc-curve.png`, `gb-pr-curve.png`, `gb-feature-importance.png`.
