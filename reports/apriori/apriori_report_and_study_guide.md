# Khai phá luật kết hợp Telco Customer Churn — Báo cáo và hướng dẫn học

Phân tích 7,043 khách; 1,869 khách churn (26.54%).

## Dữ liệu và phương pháp

Chạy ba notebook riêng theo thứ tự 01→03. Xử lý 11 TotalCharges trống sau kiểm tra tenure=0, bỏ customerID khỏi khai phá và giữ mọi khách. Dùng 12 biến phân loại, hai biến nhóm, thêm Churn. Tenure: 0–12, 13–24, 25–48, >48. Mốc phí tháng (min, phân vị 1/3, phân vị 2/3, max): [18.25, 50.4, 83.9, 118.75]. Các khoảng: [min,q1], (q1,q2], (q2,max]. Ma trận có 42 mục và 15 mục True trên mỗi khách.

Apriori: support 0,05; confidence 0,40; lift 1,5; tập mục tối đa 4, tiền đề tối đa 3; hệ quả đúng Churn=Yes. Lọc thông tin cấu trúc lặp và luật thêm điều kiện có confidence tăng không quá 2 điểm phần trăm so với luật ngắn được giữ. Giữ bảng đầy đủ để đối chiếu.

## Bảy luật trình bày

| Luật | Nhóm | Khách | Churn | Support | Confidence | Lift |
|---|---|---:|---:|---:|---:|---:|
| R1 | Hợp đồng theo tháng | 3,875 | 1,655 | 23.50% | 42.71% | 1.61 |
| R2 | Sử dụng trong 12 tháng đầu | 2,186 | 1,037 | 14.72% | 47.44% | 1.79 |
| R3 | Thanh toán bằng Electronic check | 2,365 | 1,071 | 15.21% | 45.29% | 1.71 |
| R4 | Không bảo mật trực tuyến và không hỗ trợ kỹ thuật | 2,553 | 1,250 | 17.75% | 48.96% | 1.85 |
| R5 | Cáp quang trong 12 tháng đầu | 923 | 645 | 9.16% | 69.88% | 2.63 |
| R6 | Phí tháng cao và không hỗ trợ kỹ thuật | 1,370 | 618 | 8.77% | 45.11% | 1.70 |
| R7 | Hợp đồng tháng, cáp quang, Electronic check | 1,307 | 789 | 11.20% | 60.37% | 2.27 |

Lựa chọn theo chủ đề và khả năng diễn giải; không khẳng định đứng đầu mọi tiêu chí.

### R1 — Hợp đồng theo tháng

**Luật:** Contract=Month-to-month -> Churn=Yes

**Số liệu:** 3,875 khách thỏa tiền đề; 1,655 người churn và 2,220 người không churn. Support = **23.50%**, confidence = **42.71%**, lift = **1.61**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 42.71%, bằng 1.61 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 55.02% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Một điều kiện dễ hiểu, nhóm lớn, nối trực tiếp với EDA.

**Gợi ý cần kiểm chứng:** Khảo sát lý do lựa chọn hợp đồng ngắn và mức hài lòng; cân nhắc thử nghiệm ưu đãi gia hạn tự nguyện.

**Hạn chế:** Khách chưa hài lòng có thể tự chọn hợp đồng ngắn; chưa chứng minh đổi hợp đồng sẽ giảm churn. Còn đạt ngưỡng khi tăng support lên 0,10.

### R2 — Sử dụng trong 12 tháng đầu

**Luật:** TenureGroup=0-12 -> Churn=Yes

**Số liệu:** 2,186 khách thỏa tiền đề; 1,037 người churn và 1,149 người không churn. Support = **14.72%**, confidence = **47.44%**, lift = **1.79**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 47.44%, bằng 1.79 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 31.04% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Minh họa nhóm thời gian và nhu cầu tìm hiểu giai đoạn đầu sử dụng.

**Gợi ý cần kiểm chứng:** Khảo sát trải nghiệm đăng ký, lắp đặt và sử dụng ban đầu của nhóm tenure ngắn.

**Hạn chế:** Tenure là thời lượng trong ảnh chụp dữ liệu, chưa cung cấp lịch sử theo dõi từng khách. Còn đạt ngưỡng khi tăng support lên 0,10.

### R3 — Thanh toán bằng Electronic check

**Luật:** PaymentMethod=Electronic check -> Churn=Yes

**Số liệu:** 2,365 khách thỏa tiền đề; 1,071 người churn và 1,294 người không churn. Support = **15.21%**, confidence = **45.29%**, lift = **1.71**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 45.29%, bằng 1.71 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 33.58% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Một chủ đề thanh toán, không lặp nguyên luật hợp đồng.

**Gợi ý cần kiểm chứng:** Khảo sát khó khăn và mức hài lòng với phương thức thanh toán; tìm hiểu lựa chọn thanh toán khác.

**Hạn chế:** Phương thức thanh toán có thể đi kèm loại hợp đồng hoặc thời gian sử dụng; chưa xác định nguyên nhân. Còn đạt ngưỡng khi tăng support lên 0,10.

### R4 — Không bảo mật trực tuyến và không hỗ trợ kỹ thuật

**Luật:** OnlineSecurity=No & TechSupport=No -> Churn=Yes

**Số liệu:** 2,553 khách thỏa tiền đề; 1,250 người churn và 1,303 người không churn. Support = **17.75%**, confidence = **48.96%**, lift = **1.85**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 48.96%, bằng 1.85 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 36.25% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Minh họa kết hợp hai điều kiện dịch vụ.

**Gợi ý cần kiểm chứng:** Khảo sát nhu cầu hỗ trợ và bảo mật, kiểm tra chất lượng phục vụ trước khi đề xuất gói bổ sung.

**Hạn chế:** Không đăng ký dịch vụ không đồng nghĩa đã gặp sự cố; chưa chứng minh mua thêm dịch vụ ngăn churn. Còn đạt ngưỡng khi tăng support lên 0,10.

### R5 — Cáp quang trong 12 tháng đầu

**Luật:** InternetService=Fiber optic & TenureGroup=0-12 -> Churn=Yes

**Số liệu:** 923 khách thỏa tiền đề; 645 người churn và 278 người không churn. Support = **9.16%**, confidence = **69.88%**, lift = **2.63**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 69.88%, bằng 2.63 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 13.11% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Nhóm hẹp mở rộng R2, cho phép so sánh churn của nhóm con với nhóm rộng.

**Gợi ý cần kiểm chứng:** Khảo sát trải nghiệm lắp đặt, chất lượng và kỳ vọng của khách cáp quang có tenure ngắn.

**Hạn chế:** Nhóm con của R2; chênh lệch chưa kiểm soát hợp đồng, mức phí hay các yếu tố đồng thời. Không đạt support 0,10; số khách có cả tiền đề và churn chưa tới 10% toàn bộ.

### R6 — Phí tháng cao và không hỗ trợ kỹ thuật

**Luật:** MonthlyChargesGroup=High & TechSupport=No -> Churn=Yes

**Số liệu:** 1,370 khách thỏa tiền đề; 618 người churn và 752 người không churn. Support = **8.77%**, confidence = **45.11%**, lift = **1.70**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 45.11%, bằng 1.70 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 19.45% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Đưa biến phí đã rời rạc hóa vào ví dụ kết hợp dịch vụ; giảm trùng lặp với R7.

**Gợi ý cần kiểm chứng:** Tìm hiểu giá trị cảm nhận, nhu cầu hỗ trợ và khả năng chi trả của nhóm này.

**Hạn chế:** High là nhóm phí trên phân vị 2/3 của mẫu, không phải chuẩn phí cao của ngành. Không đạt support 0,10; số khách có cả tiền đề và churn chưa tới 10% toàn bộ.

### R7 — Hợp đồng tháng, cáp quang, Electronic check

**Luật:** Contract=Month-to-month & InternetService=Fiber optic & PaymentMethod=Electronic check -> Churn=Yes

**Số liệu:** 1,307 khách thỏa tiền đề; 789 người churn và 518 người không churn. Support = **11.20%**, confidence = **60.37%**, lift = **2.27**.

**Cách hiểu:** Trong nhóm này, tỷ lệ churn quan sát là 60.37%, bằng 2.27 lần tỷ lệ chung 26.54%. Nhóm thỏa tiền đề chiếm 18.56% dữ liệu; tỷ lệ này khác support vì bao gồm cả khách không churn.

**Lý do trình bày:** Tổ hợp ba khía cạnh hợp đồng, dịch vụ và thanh toán có số khách đáng kể.

**Gợi ý cần kiểm chứng:** Khảo sát đồng thời chất lượng dịch vụ, điều khoản hợp đồng và quy trình thanh toán.

**Hạn chế:** Chồng lấn R1/R3 và có thể chồng lấn các luật khác; không cộng số khách giữa các luật. Còn đạt ngưỡng khi tăng support lên 0,10.

## Ví dụ tính tay

Với R5: 645/7043 = 9.16% là tỷ lệ toàn bộ khách vừa có tiền đề vừa churn. 645/923 = 69.88% là tỷ lệ churn trong nhóm cáp quang tenure 0–12. Chia 69.88% cho 26.54% được lift 2.63. Có 278 khách thỏa tiền đề nhưng không churn, nên luật không dự đoán chắc chắn.

R2 có confidence 47.44%; nhóm con R5 là 69.88%, chênh 22.44 điểm phần trăm. Đây là so sánh mô tả; chưa chứng minh dịch vụ cáp quang làm tăng churn.

    metric           calculation    value
   Support              645/7043 0.091580
Confidence               645/923 0.698808
      Lift (645/923)/(1869/7043) 2.633337

## Độ nhạy và chồng lấn

Ở support 0,05 có 261 luật sau lọc. Khi tăng lên 0,10, còn 137 luật sau lọc. Trong 7 luật trình bày, 5 luật còn đạt các ngưỡng trước lọc dư thừa. Hai mức support dùng cùng khách hàng và cùng mốc rời rạc hóa.

Cộng riêng kích thước 7 nhóm được 14,579 lượt khách, nhưng hợp các nhóm chỉ có 4,934 khách duy nhất (70.06% dữ liệu), trong đó 1,787 người churn. Đây là độ bao phủ mô tả trên mẫu, không phải recall của mô hình dự đoán.

![Confidence](figures/08_featured_confidence.png)

![Support và confidence](figures/09_support_confidence.png)

![Khách chồng lấn](figures/10_rule_overlap.png)

## 6. Bài học để bảo vệ

### Quy trình và vai trò từng notebook
1. Notebook 1: kiểm tra dữ liệu gốc, làm sạch, mô tả tỷ lệ và so sánh nhóm.
2. Notebook 2: biến đặc điểm thành mục giao dịch, tìm tập mục phổ biến, sinh luật, lọc và kiểm tra bằng đếm.
3. Notebook 3: chọn luật dễ giải thích, kiểm tra chồng lấn, trình bày nhận xét và gợi ý cần kiểm chứng.

### Apriori hoạt động như thế nào?
Đếm từng mục để tìm L1. Ghép các mục phổ biến để tạo ứng viên hai mục, đếm và giữ L2. Tiếp tục ba rồi bốn mục. Nếu một tập con không phổ biến, bỏ ứng viên lớn hơn chứa nó vì support không thể tăng khi thêm điều kiện. Từ các tập phổ biến, sinh luật và tính confidence/lift; trong bài chỉ giữ hệ quả Churn=Yes.

Ví dụ ba khách có A, trong đó hai khách có A và B: support(A và B) không thể lớn hơn support(A). Nguyên lý này giúp giảm số tổ hợp phải đếm. Thư viện thực hiện thuật toán; khi bảo vệ vẫn cần hiểu cơ chế.

### Mã hóa khác lựa chọn đặc trưng thế nào?
Chọn 14 biến là giới hạn phạm vi bằng ý nghĩa và khả năng diễn giải, chưa khẳng định đó là các biến quan trọng nhất. One-hot biến mỗi trạng thái thành một mục để đếm giao dịch, không đánh giá độ quan trọng của biến.

### Vì sao 15 biến thành 42 mục?
14 biến đầu vào cộng Churn là 15 biến. Mỗi biến có nhiều mức, mỗi mức thành một cột Boolean. Một khách có đúng một mức mỗi biến nên có 15 mục True trong 42 cột, không phải có 42 đặc điểm cùng lúc.

### Vì sao không chuẩn hóa hoặc SMOTE?
Apriori đếm sự có mặt của mục; số liên tục đã chia nhóm nên không cần StandardScaler. Sinh khách tổng hợp sẽ thay tần suất dữ liệu, làm support và tỷ lệ nền không còn mô tả khách thật ban đầu.

### Có mâu thuẫn với yêu cầu không ML không?
Bài dùng khai phá luật kết hợp, không huấn luyện bộ phân loại churn. Apriori đôi khi được xếp trong học không giám sát; phạm vi cần được giảng viên chấp nhận. Không gọi Apriori là thuật toán hoàn toàn ngoài ML theo mọi cách phân loại.

### Vì sao chọn support 0,05, confidence 0,40, lift 1,5?
Support 0,05 yêu cầu ít nhất 353 khách có cả tiền đề và churn trên 7.043 khách. Confidence 0,40 yêu cầu tỷ lệ churn trong nhóm từ 40%. Lift 1,5 yêu cầu tỷ lệ này bằng ít nhất 1,5 lần nền khoảng 26,5%. Các ngưỡng là lựa chọn thực nghiệm, kèm so sánh support 0,10; không có một ngưỡng chuẩn cho mọi bài.

### Luật A → churn có nghĩa tất cả khách A sẽ churn không?
Không. Confidence dưới 100% cho thấy vẫn có khách A không churn. Bài mô tả tỷ lệ trong dữ liệu, không cam kết xác suất tương lai cho từng khách.

### Lift 2 có nghĩa churn tăng 200% không?
Không. Nó nghĩa tỷ lệ nhóm bằng hai lần tỷ lệ nền; nếu diễn đạt tương đối thì cao hơn nền 100%. Phân biệt tỷ số với chênh lệch điểm phần trăm.

### Tại sao không nói nguyên nhân?
Các đặc điểm được quan sát cùng nhau; có thể có yếu tố khác tác động đồng thời. Chưa có thử nghiệm can thiệp hoặc thiết kế nghiên cứu để kết luận một thay đổi làm giảm churn.

### Tại sao không gọi luật là có ý nghĩa thống kê?
Bài chưa kiểm định giả thuyết hoặc hiệu chỉnh việc tìm nhiều luật. Support/confidence/lift là chỉ số mô tả, không thay thế p-value. Không khẳng định khác biệt ở nhóm con là có ý nghĩa thống kê.

### Làm sao biết tính toán đúng?
Kiểm tra điểm biên của nhóm, đúng một mục cho mỗi biến; ví dụ giao dịch nhỏ có đáp án tính tay; đếm lại mọi luật mục tiêu ở Notebook 2; đọc lại dữ liệu bàn giao và đối chiếu 261 luật trong Notebook 3.

### Hạn chế cần nói
Dữ liệu dạng ảnh chụp, kết quả phụ thuộc biến và mốc nhóm đã chọn, các luật được tìm và mô tả trên cùng mẫu, nhóm có thể chồng lấn, ngưỡng mang tính thực nghiệm. Chưa có bằng chứng nhân quả, hiệu quả kinh doanh hay tổng quát hóa ngoài mẫu.

### Dàn ý nói trong khoảng 3 phút
- Mục tiêu: tìm tổ hợp đặc điểm liên quan đến churn bằng Apriori.
- Nguồn và tiền xử lý: 7.043 khách, xử lý TotalCharges trống, bỏ ID, giữ tỷ lệ churn thực.
- Chuẩn bị giao dịch: 14 biến, rời rạc hóa tenure/phí tháng, thêm Churn, 42 mục Boolean.
- Thuật toán: giải thích support/confidence/lift, ngưỡng và nguyên lý tập con.
- Kết quả: số luật trước/sau lọc, trình bày R5 với số khách và ba phép tính.
- Kết luận: nhóm nào đáng khảo sát, các đề xuất cần kiểm chứng và giới hạn.

**Cách tự học:** che bảng chỉ số, tự tính R5 từ số khách; giải thích confidence bằng một câu; sau đó giải thích vì sao support có mẫu số khác. Khi làm được mới học phần lọc luật và chồng lấn.

## Tài liệu

- Nguồn cục bộ: data/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv; SHA256: 88be4b93fbe0cc83421af1c503794c97c342eca914c1576db7c276e61d61358a
- [Apriori — mlxtend](https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/apriori/)
- [Association rules — mlxtend](https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/association_rules/)
