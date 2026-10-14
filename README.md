# Telco Churn Apriori

Bài tập lớn Khai phá dữ liệu: **Khai phá luật kết hợp các đặc điểm liên quan đến việc rời bỏ dịch vụ của khách hàng viễn thông trên dữ liệu Telco Customer Churn.**

Luồng thực hiện: làm sạch và EDA → chuẩn bị giao dịch và Apriori → diễn giải luật. Mục tiêu là tìm các tổ hợp đặc điểm thường đi cùng `Churn=Yes` và gợi ý hướng khảo sát có căn cứ.

**Tiếp tục trong chat mới:** đọc [kế hoạch, tiến độ và bàn giao](PLAN.md) trước. Tài liệu ghi các quyết định đã chốt, việc đã hoàn thành, việc còn lại và cách học từng bước. Phần thực nghiệm đã có; Word, slide và luyện bảo vệ vẫn chưa hoàn thành.

## Cấu trúc

| Notebook | Nội dung |
| --- | --- |
| [01 — Làm sạch và EDA](notebooks/apriori/01_cleaning_eda.ipynb) | Kiểm tra dữ liệu, xử lý TotalCharges trống, 6 hình và bảng tỷ lệ churn |
| [02 — Giao dịch và Apriori](notebooks/apriori/02_transactions_apriori.ipynb) | Rời rạc hóa, 42 mục Boolean, sinh luật và kiểm tra số đếm |
| [03 — Diễn giải](notebooks/apriori/03_rule_interpretation.ipynb) | 7 luật tiêu biểu, 3 hình, tính tay và câu hỏi ôn bảo vệ |

- `data/raw/`: nguồn Telco có sẵn của bài tập.
- `data/processed/apriori/`: dữ liệu sạch, giao dịch và metadata.
- `reports/apriori/tables/`: bảng EDA, bảng luật đầy đủ và bảng lọc.
- `reports/apriori/figures/`: 10 hình kết quả.
- [Báo cáo và hướng dẫn học](reports/apriori/apriori_report_and_study_guide.md).

## Cài đặt và chạy trên Windows

Yêu cầu Python 3.12 (phiên bản đã kiểm tra: 3.12.14), Git nếu muốn clone.

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m ipykernel install --user --name telco-apriori --display-name "Python (Telco Apriori)"
.\.venv\Scripts\python.exe -m notebook
```

Mở lần lượt Notebook 01 → 02 → 03, chọn kernel **Python (Telco Apriori)** và Restart Kernel → Run All cho từng notebook. Notebook tìm thư mục gốc từ vị trí chạy trong project. Notebook 2 và 3 kiểm tra metadata/SHA256 đầu vào để tránh sử dụng nhầm dữ liệu.

Các notebook đã lưu output để có thể đọc trực tiếp trên GitHub. Chạy lại cập nhật các bảng, hình và báo cáo cùng tên. Thời gian chạy phụ thuộc máy và môi trường.

## Phương pháp

Dữ liệu gốc có 7.043 khách và 21 cột; bỏ cột định danh khỏi khai phá. Xử lý 11 TotalCharges trống sau khi xác nhận tenure bằng 0. Giữ mọi khách và tỷ lệ churn thực tế, không sinh mẫu cân bằng.

Chuẩn bị 12 biến phân loại, nhóm tenure và nhóm phí tháng, thêm Churn. Mỗi khách là một giao dịch với 15 mục True trong 42 mục. Tenure chia 0–12, 13–24, 25–48, >48; phí tháng chia theo phân vị 1/3 và 2/3.

Apriori dùng support 0,05, confidence 0,40, lift ≥ 1,5, tiền đề tối đa 3 điều kiện và hệ quả đúng `{Churn=Yes}`. So sánh thêm support 0,10. Giữ bảng đầy đủ, đánh dấu thông tin lặp và lọc luật thêm điều kiện nhưng confidence tăng không quá 2 điểm phần trăm so với luật ngắn được giữ.

## Kết quả

| Support | Tập mục phổ biến | Luật mục tiêu trước lọc | Đạt confidence/lift | Sau lọc dư thừa |
| --- | ---: | ---: | ---: | ---: |
| 0,05 | 11.604 | 659 | 530 | 261 |
| 0,10 | 3.717 | 221 | 195 | 137 |

Ví dụ: cáp quang và tenure 0–12 tháng → churn. Có 923 khách thỏa tiền đề, 645 người churn: support 9,16%, confidence 69,88%, lift 2,63. Bộ 7 luật trong báo cáo được chọn theo chủ đề và khả năng diễn giải.

## Giới hạn

Luật mô tả liên hệ trên cùng tập dữ liệu, chưa chứng minh nguyên nhân, hiệu quả giải pháp giữ chân hoặc khả năng dự đoán khách mới. Confidence không phải accuracy. Các nhóm luật có thể chồng lấn; không cộng số khách giữa các luật. Thay đổi ngưỡng support là kiểm tra độ nhạy, chưa xác nhận trên dữ liệu ngoài.

## Nguồn và tài liệu

File nguồn được giữ từ project học tập ban đầu: `WA_Fn-UseC_-Telco-Customer-Churn.csv`. Notebook 1 ghi SHA256 để nhận diện phiên bản. Không gán giấy phép cho dữ liệu khi chưa xác minh điều kiện nguồn.

- [Apriori — mlxtend](https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/apriori/)
- [Association rules — mlxtend](https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/association_rules/)
