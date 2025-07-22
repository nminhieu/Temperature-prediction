📌 Paris Temperature Prediction
📝 Giới thiệu
Dự án "Paris Temperature Prediction" tập trung vào việc phân tích và dự đoán nhiệt độ tại thành phố Paris dựa trên dữ liệu thời tiết trong quá khứ. Với tình hình biến đổi khí hậu ngày càng nghiêm trọng, việc hiểu rõ xu hướng nhiệt độ và dự báo chính xác là cần thiết cho quy hoạch đô thị, nông nghiệp và quản lý tài nguyên.

🎯 Mục đích và Ứng dụng
Mục đích chính:

Khám phá xu hướng nhiệt độ tại Paris trong thời gian dài.

Xây dựng mô hình dự đoán nhiệt độ dựa trên dữ liệu lịch sử.

Ứng dụng thực tế:

Hỗ trợ các cơ quan khí tượng và môi trường trong việc theo dõi biến đổi khí hậu.

Cung cấp công cụ cho các nhà hoạch định chính sách, nông dân, và nhà nghiên cứu.

Là nền tảng cho các hệ thống cảnh báo sớm hoặc mô phỏng biến đổi môi trường.

🔍 Phương pháp
Dự án “Paris Temperature Prediction” sử dụng chuỗi các bước xử lý khoa học dữ liệu, thống kê và học máy hiện đại để dự đoán nhiệt độ tại Paris theo cả ngày và tuần. Các bước chính bao gồm:

1. Kiểm tra tính dừng của chuỗi thời gian (Test for Stationarity)
Mục tiêu: xác định xem chuỗi nhiệt độ có ổn định về mặt thống kê hay không — điều kiện cần cho nhiều mô hình dự báo thời gian.

Phương pháp: Sử dụng kiểm định Augmented Dickey-Fuller (ADF Test):

Null Hypothesis (H₀): chuỗi không dừng (có xu hướng hoặc chu kỳ).

Nếu p-value < 0.05 → bác bỏ H₀ → chuỗi dừng.

2. Xử lý sai số chuỗi (Differencing Operations)
Nếu chuỗi không dừng, tiến hành lấy sai phân bậc 1 hoặc bậc 2 để ổn định trung bình.

Áp dụng:

python
Sao chép
Chỉnh sửa
df['Temp_diff'] = df['Temperature'].diff().dropna()
Mục đích: làm phẳng chuỗi, loại bỏ xu hướng tăng/giảm qua thời gian.

3. Dự báo nhanh bằng Prophet (Quick Prophet Model)
Mục tiêu: áp dụng mô hình Prophet của Meta để thử nghiệm khả năng dự báo nhanh trên chuỗi thời gian theo ngày.

Đặc điểm của Prophet:

Tự động xử lý xu hướng và mùa vụ.

Có thể thêm holidays hoặc sự kiện để cải thiện dự báo.

Cách áp dụng:

Đổi tên cột Date → ds, Temperature → y theo chuẩn Prophet.

Huấn luyện và dự báo bằng Prophet với cấu trúc đơn giản.

4. Kỹ thuật tạo đặc trưng (Feature Engineering)
Tạo thêm các đặc trưng từ cột ngày (datetime) như:

dayofweek, month, quarter, is_weekend, dayofyear, lag features,...

Lag Features: thêm các đặc trưng như nhiệt độ hôm qua, 2 ngày trước:

python
Sao chép
Chỉnh sửa
df['temp_lag1'] = df['Temperature'].shift(1)
df['temp_lag7'] = df['Temperature'].shift(7)
5. Huấn luyện mô hình theo tần suất hàng ngày (Daily Model)
🔧 Các mô hình sử dụng:
LightGBM Regressor: Gradient boosting với tốc độ nhanh và khả năng xử lý dữ liệu lớn.

Random Forest Regressor: Ensemble model mạnh mẽ với khả năng chống overfitting tốt.

🔁 Các bước:
Chia dữ liệu theo ngày làm training/test (ví dụ: train đến 2021, test 2022).

Dùng các đặc trưng đã tạo ở trên để huấn luyện mô hình.

Đánh giá kết quả qua:

MAE (Mean Absolute Error)

RMSE (Root Mean Square Error)

MAPE (Mean Absolute Percentage Error)

6. Huấn luyện mô hình theo tần suất hàng tuần (Weekly Model)
🗓 Chuẩn bị:
Gộp dữ liệu theo tuần bằng .resample('W').mean() để lấy nhiệt độ trung bình tuần.

Áp dụng lại quá trình feature engineering và tạo lag features theo tuần.

🧠 Áp dụng:
Dùng LightGBM và Random Forest để dự báo nhiệt độ tuần kế tiếp dựa vào các tuần trước.

Mô hình weekly giúp làm mượt dữ liệu và loại bỏ nhiễu ngắn hạn.

7. Trực quan hóa và đánh giá mô hình
Vẽ biểu đồ so sánh giữa nhiệt độ dự đoán và thực tế theo ngày và tuần.

So sánh độ chính xác giữa:

Prophet vs. LightGBM vs. Random Forest

Daily vs. Weekly model

Phân tích lỗi tại các mốc dị thường hoặc thời điểm model hoạt động kém hiệu quả.
