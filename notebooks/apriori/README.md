# Luồng đồ án Apriori

Đề tài: Khai phá luật kết hợp các đặc điểm liên quan đến việc rời bỏ dịch vụ của khách hàng viễn thông.

## Trạng thái

- Notebook 1: `01_cleaning_eda.ipynb` — làm sạch, bảng kiểm kê, 6 hình EDA, nhận xét và kiểm tra dữ liệu.
- Notebook 2: `02_transactions_apriori.ipynb` — rời rạc hóa, 42 mục Boolean, Apriori, kiểm tra mọi luật bằng đếm trực tiếp, lọc dư thừa và độ nhạy support.
- Notebook 3: `03_rule_interpretation.ipynb` — 7 luật biên tập, diễn giải từng luật, tính tay R5, 3 hình, độ nhạy, nhóm chồng lấn và hướng dẫn ôn bảo vệ.

Không thay đổi 8 notebook ML cũ. Không SMOTE, mô hình dự đoán hoặc phân cụm trong luồng này.

## Chạy Notebook 1

Mở bằng Jupyter/VS Code, chọn Python trong `.venv` của dự án. Restart Kernel rồi Run All. Không cần cài thêm thư viện cho Notebook 1 nếu dùng môi trường hiện có. Mở notebook bên trong thư mục dự án để tìm đúng nguồn dữ liệu.

Nguồn: `data/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv`.
Đầu ra riêng: `data/processed/apriori/` và `reports/apriori/{tables,figures}/`.
Chạy lại ghi cập nhật các đầu ra Apriori cùng tên; không sửa file gốc hoặc kết quả ML.

## Chạy Notebook 2

Chạy Notebook 1 trước để tạo bảng sạch và metadata. Dùng cùng kernel Python của dự án, Restart Kernel rồi Run All cho Notebook 2. Notebook tự kiểm tra SHA256 đầu vào; nếu bảng sạch thay đổi, chạy lại Notebook 1 trước.

Thư viện bổ sung được ghim trong `requirements-apriori.txt`: `mlxtend==0.25.0`. Đã cài vào `.venv` hiện tại, không nâng cấp thư viện ML cũ. Nếu dựng môi trường khác, cài requirements gốc rồi cài file bổ sung này.

Kết quả lần chạy đã kiểm tra:

| Support | Tập mục phổ biến | Luật mục tiêu trước lọc | Đạt confidence/lift | Sau lọc dư thừa |
| --- | ---: | ---: | ---: | ---: |
| 0,05 | 11.604 | 659 | 530 | 261 |
| 0,10 | 3.717 | 221 | 195 | 137 |

Đây là kết quả mô tả cùng tập dữ liệu, không phải điểm dự đoán. Bảng đầy đủ và bảng lọc nằm trong `reports/apriori/tables/apriori_*.csv`; metadata và bảng giao dịch nằm trong `data/processed/apriori/`. Không cộng số khách giữa các luật vì các nhóm có thể chồng lấn.

Danh sách để trình bày nằm trong Notebook 3 và `reports/apriori/tables/apriori_featured_rules.csv`. Bảng 12 luật đầu trong Notebook 2 chỉ là xếp hạng số học.

## Chạy Notebook 3 và học bài

Sau Notebook 1 và 2, mở Notebook 3 bằng cùng kernel, Restart Kernel rồi Run All. Notebook đọc lại bảng giao dịch và đối chiếu nguồn, số khách và chỉ số của 261 luật sau lọc trước khi chọn 7 luật.

Đọc mục 1 để hiểu lý do chọn luật, mục 2 để đọc từng diễn giải, mục 3 để tự tính R5 và mục 6 để ôn bảo vệ. 7 luật là lựa chọn trình bày theo chủ đề, không phải các phân khúc độc lập hoặc danh sách tốt nhất theo mọi tiêu chí.

Báo cáo học tập: `reports/apriori/apriori_report_and_study_guide.md`. Có số liệu, giải thích support/confidence/lift, ví dụ R5, đề xuất cần kiểm chứng, hạn chế và dàn ý nói trong khoảng 3 phút. Đây là Markdown, chưa phải bản Word/PPT.

Notebook 3 xuất ba hình mới trong `reports/apriori/figures/`: confidence của 7 luật, support/confidence của các luật và số khách chồng lấn. Không cộng số khách giữa các luật.

Ba notebook đã triển khai. Trước khi soạn bài nộp, xác nhận phạm vi Apriori được giảng viên chấp nhận và đọc các giới hạn trong báo cáo.

## Quy ước khai phá

- 12 biến phân loại: Contract, InternetService, TechSupport, OnlineSecurity, OnlineBackup, DeviceProtection, StreamingTV, PaymentMethod, PaperlessBilling, SeniorCitizen, Partner, Dependents.
- Cộng TenureGroup và MonthlyChargesGroup, tổng 14 biến đầu vào; thêm Churn để tạo giao dịch.
- Tenure: 0–12, 13–24, 25–48, >48; phí tháng chia 3 nhóm phân vị, công bố mốc thực tế.
- Không đưa TotalCharges vào luật chính để hạn chế thông tin chồng lấn; không khẳng định TotalCharges hoàn toàn tương đương tenure và phí tháng.
- Support 0,05; confidence 0,40; lift >= 1,5; tiền đề tối đa 3 mục, không chứa Churn; hệ quả đúng Churn=Yes. Tập mục tối đa 4 mục.
- Bảng đầy đủ giữ nguyên. Bảng trình bày đánh dấu luật dư thừa nếu confidence tăng không quá 2 điểm phần trăm so với luật tiền đề tập con, cùng hệ quả.
- So sánh support 0,10 là độ nhạy ngưỡng, không phải bằng chứng tổng quát hóa ngoài mẫu.
- Không bắt buộc tìm đủ 5–8 luật. Không diễn giải liên hệ thành nhân quả.

Apriori cần được giảng viên chấp nhận trong phạm vi môn học; luồng này không tạo bộ phân loại churn.
