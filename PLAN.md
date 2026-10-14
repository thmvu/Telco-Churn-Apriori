# Kế hoạch, tiến độ và bàn giao đồ án Telco Churn Apriori

Cập nhật: 15/10/2026. Đây là tài liệu để tiếp tục trong một chat mới. Các ô đã đánh dấu phản ánh công việc đã thực hiện, không phải dự định.

## 1. Mục tiêu và bối cảnh

Đây là bài tập lớn môn Khai phá dữ liệu, mức đồ án sinh viên, chưa phải sản phẩm thương mại. Người học muốn hiểu quy trình và giải thích được khi giảng viên hỏi; cần hướng dẫn tiếng Việt từng ý nhỏ, dùng ví dụ đếm khách trước khi đưa công thức.

Tên đề tài: **Khai phá luật kết hợp các đặc điểm liên quan đến việc rời bỏ dịch vụ của khách hàng viễn thông (Telco Customer Churn).** Dùng “liên quan” thay cho “ảnh hưởng” hoặc “nguyên nhân”.

Ba câu hỏi nghiên cứu:

1. Nhóm khách hàng nào có tỷ lệ churn cao trong dữ liệu?
2. Những tổ hợp đặc điểm nào thường đi cùng `Churn=Yes`?
3. Các luật có dễ hiểu và đủ căn cứ để gợi ý hướng tìm hiểu tiếp không?

Luồng đã chốt: **làm sạch và EDA → chuẩn bị giao dịch và Apriori → diễn giải luật → báo cáo và bảo vệ**.

Apriori phù hợp vì nhiều thuộc tính phân loại, mục tiêu tìm tổ hợp đồng xuất hiện và kết quả dạng luật dễ giải thích. Không khẳng định Apriori hoàn toàn nằm ngoài ML theo mọi phân loại: thuật toán này đôi khi được xếp vào học không giám sát. Việc giảng viên chấp nhận hướng Apriori vẫn chưa được xác nhận.

## 2. Phân biệt hai project

- Project này: `Telco-Churn-Apriori`, repository Public: https://github.com/thmvu/Telco-Churn-Apriori.
- Thư mục local khi bàn giao: `D:/vu/hoctap/Telco-Churn-Apriori`.
- Project cũ: `D:/vu/hoctap/Customer-Churn-Analysis`, repository khác, chứa luồng ML và ensemble. Người dùng có thể tiếp tục nó trong chat cũ.
- Đã sao chép phần Apriori thành project độc lập, không xóa bản ở project cũ. Không sửa, stage, commit hoặc push project cũ khi thực hiện công việc của kế hoạch này.
- Không mang các số liệu AdaBoost, XGBoost, ensemble, threshold, accuracy/recall của project cũ vào báo cáo Apriori.

Project này không thực hiện mô hình dự đoán churn, SMOTE/SMOTENC, tuning ML, phân cụm hoặc demo dự đoán khách mới. Ý tưởng K-means/K-Prototypes đã loại khỏi phạm vi, không tự thêm lại.

## 3. Cấu trúc đã thực hiện

| Tệp/thư mục | Vai trò |
| --- | --- |
| `notebooks/apriori/01_cleaning_eda.ipynb` | Làm sạch, 6 hình EDA và bảng nhóm |
| `notebooks/apriori/02_transactions_apriori.ipynb` | Rời rạc hóa, Apriori, kiểm tra luật và độ nhạy |
| `notebooks/apriori/03_rule_interpretation.ipynb` | 7 luật, diễn giải, 3 hình và ôn bảo vệ |
| `data/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv` | Dữ liệu nguồn |
| `data/processed/apriori/` | Bảng sạch, giao dịch và metadata |
| `reports/apriori/tables/` | Bảng EDA, tập mục, luật đầy đủ và luật lọc |
| `reports/apriori/figures/` | 10 hình PNG |
| `reports/apriori/apriori_report_and_study_guide.md` | Báo cáo Markdown và hướng dẫn học |
| `requirements.txt` | Phụ thuộc của project độc lập |
| `README.md` | Hướng dẫn tổng quan và cài đặt |

Mọi logic phân tích nằm trong các cell notebook, không cần module Python nội bộ của project cũ. Không tạo thêm notebook chỉ để chia nhỏ các bước đã có.

## 4. Notebook 1: làm sạch và EDA — đã hoàn thành

- [x] Đọc nguồn, kiểm tra schema, ô trống, dòng trùng và mã khách lặp.
- [x] Kiểm tra dữ liệu gốc 7.043 dòng, 21 cột.
- [x] Cắt khoảng trắng, chuyển tenure/MonthlyCharges/TotalCharges sang số.
- [x] Kiểm tra 11 TotalCharges trống đều có tenure=0, sau đó điền 0. Ghi rõ là quy tắc xử lý cho đồ án.
- [x] Bỏ customerID khỏi khai phá; giữ mọi khách. Không tự xóa khách khác mã chỉ vì hồ sơ giống nhau.
- [x] Đổi SeniorCitizen 0/1 thành No/Yes; giữ No internet service và No phone service là trạng thái hợp lệ.
- [x] Giữ nguyên 5.174 khách No, 1.869 khách Yes, tỷ lệ churn 26,536987%.
- [x] Bảng nhóm có số khách, số churn, tỷ lệ churn; đánh dấu nhóm ít khách.
- [x] Sáu hình: phân bố churn, Contract, nhóm tenure, InternetService, TechSupport/OnlineSecurity, MonthlyCharges theo churn.
- [x] Nhận xét dưới hình được tính từ dữ liệu, không lấy số liệu từ ảnh hoặc kết luận định sẵn.
- [x] Kiểm tra tổng số khách/churn, điểm biên tenure, một nhóm bằng đếm trực tiếp, CSV đọc lại và SHA256 nguồn.

Đầu ra chính: `telco_clean.csv`, `cleaning_metadata.json`, các bảng EDA và hình 01–06. Bảng sạch có 20 cột, vẫn giữ tenure/phí ở dạng số. EDA dùng toàn bộ dữ liệu, không train/test vì đây là luồng khám phá mô tả.

## 5. Notebook 2: giao dịch và Apriori — đã hoàn thành

### Chuẩn bị giao dịch

- [x] 12 biến phân loại: Contract, InternetService, TechSupport, OnlineSecurity, OnlineBackup, DeviceProtection, StreamingTV, PaymentMethod, PaperlessBilling, SeniorCitizen, Partner, Dependents.
- [x] Thêm TenureGroup và MonthlyChargesGroup: tổng 14 biến đầu vào, thêm Churn thành 15 biến giao dịch.
- [x] Tenure: 0–12, 13–24, 25–48, >48 tháng; không chồng lấn.
- [x] Phí tháng chia theo phân vị 1/3 và 2/3, không dùng Churn để chọn mốc.
- [x] Mốc thực tế: Low [18,25; 50,40], Medium (50,40; 83,90], High (83,90; 118,75]. Lưu mốc số đầy đủ trong metadata.
- [x] Không dùng TotalCharges trong luật chính để hạn chế thông tin chồng lấn; không khẳng định nó hoàn toàn tương đương tenure và phí tháng.
- [x] Mã hóa Boolean đầy đủ, không drop_first: 42 mục, mỗi khách có đúng 15 mục True.
- [x] Chạy trên toàn bộ khách, không lọc chỉ người churn hoặc cân bằng dữ liệu.

### Ngưỡng và lọc luật

| Thiết lập | Giá trị |
| --- | --- |
| Support chính | 0,05 |
| Support đối chiếu | 0,10 |
| Confidence tối thiểu | 0,40 |
| Lift tối thiểu | 1,5 |
| Tiền đề | 1–3 mục, không chứa mục Churn |
| Hệ quả | đúng `{Churn=Yes}` |
| Tập mục tối đa | 4 mục |
| Dư thừa | confidence tăng không quá 0,02 so với luật ngắn được giữ, tiền đề tập con và cùng hệ quả |

- [x] Tìm tập mục phổ biến bằng `mlxtend.apriori`, sau đó sinh luật bằng `association_rules`.
- [x] Lưu mọi luật mục tiêu đạt support, kể cả chưa đạt confidence/lift, kèm cờ lọc.
- [x] Thêm số khách thỏa tiền đề và số churn trong nhóm.
- [x] Đếm lại mọi luật mục tiêu từ bảng giao dịch để đối chiếu support/confidence/lift.
- [x] Kiểm tra bằng bộ giao dịch nhỏ có đáp án tính tay và trường hợp không có luật.
- [x] Kiểm tra điều kiện dư thừa đúng 2 điểm phần trăm, không nhầm 2% tương đối.
- [x] Đánh dấu tiền đề lặp nhiều trạng thái “không có Internet” sau khi kiểm tra các cột tương đương.
- [x] Chạy lại support 0,10; đối chiếu chỉ số của các luật chung.
- [x] Có ví dụ tính tay luật Contract=Month-to-month → Churn=Yes.
- [x] Một hình số luật theo ngưỡng support (hình 07).

| Support | Tập mục phổ biến | Luật mục tiêu trước lọc | Đạt confidence/lift | Sau lọc dư thừa |
| --- | ---: | ---: | ---: | ---: |
| 0,05 | 11.604 | 659 | 530 | 261 |
| 0,10 | 3.717 | 221 | 195 | 137 |

Đây là số luật trên cùng mẫu; tăng support là kiểm tra độ nhạy, không phải kiểm chứng trên khách mới. Không tự hạ ngưỡng liên tục để tìm luật đẹp.

## 6. Notebook 3: diễn giải và học bài — đã hoàn thành

- [x] Kiểm tra nguồn giao dịch bằng cách tái dựng từ bảng sạch và mốc nhóm.
- [x] Đối chiếu 261 luật đã lọc bằng số khách thực tế.
- [x] Chọn 7 luật theo chủ đề, số khách và khả năng giải thích, không chỉ lấy 7 lift cao nhất.
- [x] Mỗi luật có: luật → số liệu → cách hiểu → lý do chọn → gợi ý cần kiểm chứng → hạn chế.
- [x] Kiểm tra chồng lấn. Đã thay ví dụ phí cao/hợp đồng tháng/Electronic check vì gần trùng nhóm với R7 (786/789 khách), bằng ví dụ phí cao/không TechSupport.
- [x] Có phép tính tay R5 và so sánh mô tả nhóm rộng R2 với nhóm con R5.
- [x] Ba hình: confidence của 7 luật, support/confidence của các luật, số khách chồng lấn (hình 08–10).
- [x] Có độ nhạy của 7 luật; 5/7 luật còn đạt ngưỡng trước lọc dư thừa khi support lên 0,10.
- [x] Xuất bảng luật trình bày và báo cáo Markdown có giải thích, câu hỏi bảo vệ, dàn ý nói khoảng 3 phút.

| Luật | Tiền đề | Khách thỏa | Churn trong nhóm | Confidence | Lift |
| --- | --- | ---: | ---: | ---: | ---: |
| R1 | Contract=Month-to-month | 3.875 | 1.655 | 42,71% | 1,61 |
| R2 | TenureGroup=0-12 | 2.186 | 1.037 | 47,44% | 1,79 |
| R3 | PaymentMethod=Electronic check | 2.365 | 1.071 | 45,29% | 1,71 |
| R4 | OnlineSecurity=No và TechSupport=No | 2.553 | 1.250 | 48,96% | 1,85 |
| R5 | InternetService=Fiber optic và TenureGroup=0-12 | 923 | 645 | 69,88% | 2,63 |
| R6 | MonthlyChargesGroup=High và TechSupport=No | 1.370 | 618 | 45,11% | 1,70 |
| R7 | Contract=Month-to-month, InternetService=Fiber optic, PaymentMethod=Electronic check | 1.307 | 789 | 60,37% | 2,27 |

Ví dụ R5: support = 645/7.043 = 9,16%; confidence = 645/923 = 69,88%; lift = (645/923)/(1.869/7.043) = 2,63. Có 278 khách thỏa tiền đề nhưng không churn. R5 bị loại khi support lên 0,10 dù confidence cao, vì support chỉ 9,16%.

Các nhóm chồng lấn. Không cộng kích thước nhóm để suy ra số khách riêng biệt. Bảng `featured_rule_customer_overlap.csv` đếm khách thỏa tiền đề của cả hai luật.

## 7. Kiểm chứng và Git đã thực hiện

- [x] Tách project độc lập bằng cách sao chép riêng phần Apriori và dữ liệu gốc; giữ project cũ.
- [x] Chạy lại cả ba notebook trong thư mục project mới, không lỗi.
- [x] Output đã lưu: Notebook 1 có 11 cell code/6 hình; Notebook 2 có 8 cell code/1 hình; Notebook 3 có 7 cell code/3 hình. Không có cell code chưa chạy hoặc output lỗi tại thời điểm bàn giao.
- [x] Kiểm tra bằng số đếm, phép tính tay và đọc lại bảng xuất.
- [x] Xem hình để kiểm tra nhãn, bố cục và số liệu.
- [x] Tạo README, requirements và gitignore riêng.
- [x] Tạo repo Public thuộc `thmvu`, push nhánh `main` và xác nhận SHA remote.

Commit đầu chứa project: `a967daf` (`feat: add standalone Telco Apriori data mining project`). Các commit tài liệu sau có thể mới hơn; kiểm tra `git log` thay vì coi mã này luôn là HEAD.

Môi trường đã dùng: Python 3.12.14, pandas 3.0.3, mlxtend 0.25.0. Lần kiểm chứng dùng interpreter `.venv` sẵn có của project cũ để chạy notebook với thư mục làm việc mới. **Project mới chưa được kiểm chứng bằng một `.venv` mới cài từ đầu.** Hướng dẫn cài độc lập có trong README; cần phân biệt chạy độc lập về dữ liệu/code với kiểm tra cài môi trường sạch.

SHA256 nguồn: `88be4b93fbe0cc83421af1c503794c97c342eca914c1576db7c276e61d61358a`.

## 8. Công việc còn lại

### Trước khi tạo bài nộp

- [ ] Người dùng xác nhận giảng viên cho phép Apriori trong phạm vi đề bài. Nếu chưa có thông tin, ghi rõ đang chờ, không khẳng định đã được duyệt.
- [ ] Người dùng cung cấp yêu cầu nộp: mẫu báo cáo, số trang, thời lượng nói, số slide, thành viên, môn/lớp và hạn nộp nếu có. Không bịa thông tin trang bìa hoặc tài liệu nguồn.
- [ ] Kiểm tra môi trường cài mới từ requirements nếu cần chứng minh khả năng tái tạo trên máy khác; không cần tìm lại toàn bộ thuật toán.
- [ ] Xác minh nguồn phân phối/điều kiện sử dụng dataset trước khi thêm giấy phép cho dữ liệu. Hiện chỉ ghi nguồn file cục bộ và SHA256, chưa gán giấy phép.

### Báo cáo và slide

- [ ] Soạn báo cáo Word theo nội dung Markdown đã có, kiểm tra bố cục và hình.
- [ ] Soạn slide theo cùng số liệu; gợi ý khoảng 10–12 slide nếu người dùng chưa có yêu cầu cụ thể.
- [ ] Đối chiếu bảng, hình và báo cáo/slide để không trộn số liệu hoặc phiên bản.
- [ ] Hoàn thiện phần cơ sở lý thuyết và trích dẫn tài liệu chính thức Apriori/association rules.

Cấu trúc bài nộp:

1. Giới thiệu bài toán và câu hỏi nghiên cứu.
2. Dữ liệu và làm sạch.
3. EDA: chọn các phát hiện chính thay vì đưa hết mọi bảng.
4. Cơ sở lý thuyết: giao dịch, tập mục, Apriori, support/confidence/lift, nguyên lý tập con.
5. Thực nghiệm: rời rạc hóa, ngưỡng, lọc luật, bảng kết quả, ví dụ tính tay và độ nhạy.
6. Diễn giải và gợi ý cần kiểm chứng.
7. Hạn chế và hướng tiếp theo.

### Học và luyện bảo vệ

- [ ] Người dùng giải thích được một dòng dữ liệu và nhãn Churn.
- [ ] Giải thích được vì sao 15 biến giao dịch thành 42 mục, nhưng mỗi khách chỉ có 15 mục True.
- [ ] Tự tính R5 từ N=7.043, c=1.869, a=923, ac=645.
- [ ] Phân biệt mẫu số support và confidence; giải thích lift bằng một câu.
- [ ] Giải thích được thuật toán loại tổ hợp theo nguyên lý Apriori.
- [ ] Giải thích ngưỡng thực nghiệm và vì sao R5 không còn ở support 0,10.
- [ ] Giải thích được luật dư thừa, nhóm chồng lấn và không cộng số khách.
- [ ] Trình bày ít nhất một luật theo mẫu 5 bước, không kết luận nhân quả.
- [ ] Luyện nói 3 phút, rồi hỏi đáp theo yêu cầu bảo vệ thực tế.

Không đánh dấu người dùng đã hiểu chỉ vì tài liệu đã được viết. Trong chat gần nhất, người dùng nói chưa hiểu và cần giải thích chậm.

## 9. Cách dạy trong chat mới

Bắt đầu bằng ví dụ giả định: 100 khách, 20 churn; nhóm khách mới dùng cáp quang có 10 người, 6 churn. Giải thích 20/100 là tỷ lệ chung, 6/10 là tỷ lệ churn của nhóm. Khi người dùng hiểu mẫu số mới chuyển sang support 6/100 và lift (6/10)/(20/100).

Mỗi lượt một ý chính, dùng số nhỏ trước số thực tế. Tránh đưa liên tiếp hàng loạt thuật ngữ. Sau đó mới mở mục tương ứng trong notebook. Không yêu cầu học thuộc code; cần hiểu mục đích, đầu vào, đầu ra và quyết định.

Câu hỏi bảo vệ cần ôn:

1. Vì sao chọn Apriori cho mục tiêu này?
2. Support, confidence, lift khác nhau ở đâu?
3. Vì sao giữ cả khách churn và không churn khi khai phá?
4. Vì sao không SMOTE hoặc chuẩn hóa số cho Apriori?
5. Vì sao chia nhóm biến số và mốc chia đã chọn thế nào?
6. Vì sao confidence không phải accuracy dự đoán?
7. Vì sao chọn ngưỡng này, đổi ngưỡng thì sao?
8. Vì sao luật dài hơn có thể dư thừa?
9. Vì sao không cộng số khách giữa các luật?
10. Vì sao không suy ra nguyên nhân hoặc cam kết hiệu quả giải pháp?

## 10. Nguyên tắc khi tiếp tục

- Đọc `PLAN.md`, `README.md` và báo cáo học tập trước khi thay đổi. Kiểm tra Git status và notebook thực tế vì tiến độ có thể đã thay đổi.
- Tiếp tục công việc chưa hoàn thành theo lệnh mới; không tự xây lại 3 notebook hoặc chạy tìm luật khác chỉ vì sang chat mới.
- Giữ các ngưỡng đã chốt nếu người dùng chưa yêu cầu thay đổi. Khi đổi, ghi lý do và cập nhật mọi tài liệu chịu ảnh hưởng.
- Bảng đầy đủ là bằng chứng. Không chỉ giữ luật đẹp hoặc xóa luật không phù hợp câu chuyện.
- Không tự thêm ML, phân cụm, SMOTE hoặc thay nhiệm vụ thành dự đoán khách hàng mới.
- Đề xuất kinh doanh là hướng khảo sát/thử nghiệm; không bịa mức giảm churn, lợi nhuận hoặc nguyên nhân.
- Các ngưỡng/chỉ số chưa thay thế kiểm định thống kê. Bài hiện chưa có kiểm định Chi-square hoặc kết luận ý nghĩa thống kê.
- Chỉ commit/push thay đổi đúng project này khi nằm trong yêu cầu người dùng; không force push hoặc đẩy thay đổi project cũ.
- Cập nhật checklist này khi thực sự hoàn thành việc còn lại.

## 11. Lệnh khởi đầu gợi ý cho chat mới

> Đây là project Telco-Churn-Apriori. Hãy đọc PLAN.md, README.md và reports/apriori/apriori_report_and_study_guide.md, kiểm tra tiến độ hiện tại rồi tiếp tục theo yêu cầu của tôi. Giải thích tiếng Việt từng ý nhỏ vì tôi chưa nắm vững Apriori. Không làm lại project ML cũ và không tự thay các ngưỡng đã chốt.

Sau câu đó, người dùng có thể chỉ định “dạy tôi support/confidence/lift”, “làm báo cáo Word” hoặc “làm slide”.
