# Data And AI Flow

Flow dữ liệu và huấn luyện AI:

```text
Self-collected data
        |
        v
Preprocessing: kiểm tra lỗi, lọc nhiễu, chuẩn hóa
        |
        v
Windowing: 100 mẫu ở 50Hz
        |
        v
Feature extraction: 10 nhóm đặc trưng trên 6 trục + SMV features
        |
        v
LOSO split: mỗi fold giữ một người làm test
        |
        v
Hyperparameter tuning
        |
        v
Evaluate RF, Gradient Boosting, CNN-LSTM-MHA
        |
        v
Select Gradient Boosting for deployment
```

Ảnh chính: `../../images-report/pipeline-training-loso.png`.

Ảnh kết quả: `../../images-report/compare-three-models.png`.
