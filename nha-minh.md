# Ý tưởng khởi nghiệp: Nền tảng kết nối quán địa phương với khách hàng

**Tóm tắt một câu:** Nền tảng địa phương giúp quán ăn, tạp hóa và tiệm làm tại nhà tiếp cận khách xung quanh với giá gốc. Nền tảng **không sống bằng hoa hồng trên từng đơn** mà bằng công cụ quản lý, quảng bá và thuê bao, đồng thời hỗ trợ nhiều cách giao hàng, kể cả cho quán chưa có shipper.

---

## S: Situation (Hiện trạng)

- Nhiều quán ăn, tạp hóa, tiệm làm tại nhà đã có khách quen và tự giao được trong khu vực gần.
- Các app giao hàng lớn giúp quán có thêm đơn nhưng thu hoa hồng cao. Nhiều quán tăng giá trên app để bù, nên khách trả nhiều hơn so với mua trực tiếp (mức hoa hồng và chênh giá cần kiểm chứng bằng số liệu thực tế).
- Nhiều quán bán qua Zalo, Facebook, nhóm khu phố: rẻ nhưng khó tìm, không có hệ thống đặt hàng, thanh toán, đánh giá tập trung.
- Một phần quán chưa có shipper nên không dám mở bán online.
- Đã có tiền lệ thị trường cho hướng "ít hoặc không hoa hồng": ChowNow (Mỹ) thu phí thuê bao cố định thay vì hoa hồng; ONDC và magicpin (Ấn Độ) tách nền tảng để giảm hoa hồng. Tuy vậy, chưa thấy ví dụ thành công rõ ràng cho phần quán tự giao theo khung giờ hoặc shipper khu vực.

## C: Complication (Mâu thuẫn / vấn đề)

- **Quán:** Muốn thêm đơn nhưng không muốn mất phần lớn lợi nhuận cho nền tảng. Tự bán thì thiếu kênh tiếp cận khách mới.
- **Khách:** Muốn mua ở quán quen với giá gốc nhưng không có nơi nào gom các quán ở gần mình. Chọn app thì đắt hơn, chọn nhắn tin thì bất tiện.
- **Quán chưa có shipper:** Bị loại khỏi cuộc chơi online, hoặc phải chịu hoa hồng cao.
- **Rủi ro rò rỉ (khách và quán "bỏ app" sang Zalo):** Nền tảng không nắm khâu giao hàng và thanh toán, số Zalo của quán vốn công khai, đơn ăn uống lặp lại thường xuyên. Nếu thu hoa hồng trên đơn, quán sẽ có động cơ né rất rõ.
- **Thách thức của người làm:** "gà – quả trứng", thói quen khách khó đổi, rào cản công nghệ thấp, giả định "phí ship rẻ nhất" chưa kiểm chứng, và bài học từ Dunzo (mở rộng logistics quá sớm khi lợi nhuận mỗi đơn chưa vững).

## Q: Question (Câu hỏi then chốt)

> Làm thế nào để xây một nền tảng địa phương giúp quán nhỏ (kể cả quán chưa có shipper) tiếp cận thêm khách với chi phí thấp, khách được giá gốc, và nền tảng vẫn thu được tiền ổn định **ngay cả khi khách và quán có lúc giao dịch trực tiếp với nhau**?

Các câu hỏi con:

1. Quán có sẵn lòng trả phí thuê bao hoặc phí quảng bá không, bao nhiêu?
2. Công cụ nào khiến quán dùng hằng ngày và khách quay lại qua nền tảng?
3. Quán chưa có shipper có chịu tự giao theo khung giờ không?
4. Làm sao có quán và khách đầu tiên mà không có ngân sách quảng cáo?

## A: Answer (Giải pháp)

### 1. Định vị

"Chợ online của quán địa phương": **giá gốc, quán tự giao hoặc khách tự lấy, không ăn hoa hồng trên từng đơn**. Lợi thế cạnh tranh nằm ở mạng lưới quán và cộng đồng từng khu vực, không nằm ở công nghệ.

### 2. Mô hình doanh thu

Nguyên tắc: **không đặt cược doanh thu vào hoa hồng trên đơn.** Nếu khách nhắn thẳng quán thì nền tảng không mất khoản phần trăm nào.

- **Phí thuê bao cố định** hằng tháng cho quán (gói thấp, có gói miễn phí giai đoạn đầu).
- **Phí quảng bá:** đẩy quán lên đầu danh sách trong khu vực, hiển thị ưu đãi.
- **Công cụ quản lý** trả phí ở gói cao hơn (thống kê khách, chăm sóc khách, thẻ tích điểm nâng cao).
- **Phí giao hàng (lớp C)** chỉ là phí dịch vụ trả cho shipper theo bậc khu vực hoặc khung giờ, không phải hoa hồng.
- Phí xử lý thanh toán nếu đưa thanh toán vào app.

### 3. Chiến lược giữ người dùng ở lại (chống rò rỉ)

Nguyên tắc: **biến việc ở lại thành lợi ích thật, không ép buộc.** Chấp nhận một phần rò rỉ là bình thường, vì thứ nền tảng thu tiền là thứ khó tự làm ngoài nền tảng: khách mới, quảng bá và công cụ.

**Cho quán (dùng hằng ngày):**

- Quản lý đơn và menu, bật/tắt món, khung giờ giao, bán kính giao.
- Thống kê khách: ai hay mua, món nào bán chạy, giờ nào đông.
- Nhắc khách đặt lại, gửi ưu đãi cho khách quen.
- Thẻ tích điểm cho quán.

**Cho khách (lý do quay lại):**

- Tích điểm hoặc voucher chỉ dùng được trong app.
- Đặt lại một chạm, lưu lịch sử đơn.
- Đánh giá và xếp hạng quán trong khu vực.

**Thanh toán trong app (nếu làm được, giai đoạn sau):** thêm lớp bảo vệ khách khi đơn sai hoặc trễ, điều mà nhắn Zalo không có. Đây là chức năng nặng nên chỉ triển khai khi đã có lượng đơn ổn định.

**Đo rò rỉ từ sớm:** theo dõi tỷ lệ khách đặt lại qua nền tảng để biết mô hình có thu được tiền thật không trước khi mở rộng.

### 4. Sản phẩm cốt lõi

- Trang/app liệt kê quán gần khách, menu, giá, bán kính giao, khung giờ giao.
- Khách đặt hàng, chọn giao tận nơi hoặc **tự đến lấy (pickup)**.
- Bộ công cụ quản lý cho quán và các tính năng giữ khách nêu ở mục 3.

### 5. Ba lớp giải pháp giao hàng

| Lớp | Đối tượng | Cách hoạt động | Giai đoạn |
|---|---|---|---|
| **A. Quán tự giao / khách tự lấy** | Quán đã có shipper hoặc khách ở gần | Giá gốc, không tốn logistics cho nền tảng | MVP |
| **B. Gom đơn giao theo khung giờ** | Quán chưa có shipper | Quán chọn 1–3 khung giờ giao mỗi ngày, hệ thống gom đơn theo khu vực và gợi ý lộ trình, quán giao một lượt. Có ngưỡng đơn tối thiểu mỗi khung giờ | Giai đoạn 2 |
| **C. Thuê shipper theo khu vực** (ý tưởng phụ) | Quán không muốn hoặc không thể tự giao | Nhóm shipper tự do phục vụ một khu, trả theo đơn hoặc theo khung giờ | Giai đoạn 3, chỉ khi B chạy ổn |

- Lớp B phù hợp với tạp hóa, bánh, trái cây, đồ làm sẵn và cơm hộp đặt trước; không hợp món cần ăn nóng ngay.
- Cách thuyết phục quán tự giao: "chạy một vòng thay vì chạy từng đơn", "biết trước số đơn để chuẩn bị hàng", "không mất hoa hồng lớn".
- Lớp C chỉ mở sau khi B chạy ổn, vì đây là bước vào logistics. Cách thử rủi ro thấp: một khu, 2–3 shipper tự do trả theo đơn, một khung giờ mỗi ngày, phí ship khách trả theo bậc dễ hiểu.

### 6. Go-to-market: khu vực nhỏ + nội dung tự nhiên

- Chọn **một khu** (một phường, chợ hoặc khu dân cư) và tuyển 20–30 quán đầu tiên trước khi mở rộng.
- Xây kênh bằng nội dung, không cần quảng cáo (chi tiết ở Phụ lục A).
- Mỗi nội dung có lời kêu gọi hành động rõ (ví dụ "nhắn mình nếu quán bạn muốn thử").

### 7. Lộ trình

| Giai đoạn | Việc chính | Điều cần chứng minh |
|---|---|---|
| **1. Kiểm chứng (1–2 tháng)** | Phỏng vấn 20–30 quán và 30–50 khách, chạy thử thủ công qua Zalo/web đơn giản với 10–20 quán, bắt đầu đăng nội dung | Quán chịu tham gia và trả phí, khách đặt lại, có đơn đều |
| **2. Sản phẩm nhỏ** | Trang đặt hàng chính thức, pickup, gom đơn theo khung giờ, công cụ quản lý đơn/menu, thẻ tích điểm, thu phí thuê bao thử nghiệm | Quán dùng công cụ hằng ngày, quán chưa có shipper chịu tự giao theo khung giờ |
| **3. Mở rộng** | Thanh toán trong app (nếu khả thi), thử shipper khu vực trong một khu, nhân rộng khu mới | Chi phí giao mỗi đơn thấp hơn mức khách chịu trả, tỷ lệ rò rỉ chấp nhận được |

### 8. Chỉ số theo dõi

- Số quán hoạt động mỗi tuần, số đơn mỗi quán, tỷ lệ quán chịu trả phí thuê bao.
- **Tỷ lệ khách đặt lại qua nền tảng** (chỉ số rò rỉ quan trọng nhất) và mức độ quán dùng công cụ hằng ngày.
- Số đơn mỗi khung giờ và chi phí giao trung bình mỗi đơn.
- Chi phí thời gian để có một quán mới, và tỷ lệ người xem nội dung chuyển thành đăng ký.

### 9. Rủi ro chính và cách giảm

| Rủi ro | Cách giảm |
|---|---|
| Khách và quán "bỏ app" sang Zalo | Không thu hoa hồng trên đơn, cho quán công cụ dùng hằng ngày, tích điểm và đặt lại một chạm cho khách, thanh toán trong app sau này |
| Quán nhỏ khó trả phí thuê bao | Gói miễn phí giai đoạn đầu, thu sau khi công cụ chứng minh có giá trị, chỉ thu quảng bá cho quán muốn thêm khách |
| Gà – quả trứng | Chỉ làm một khu nhỏ, tuyển quán trước bằng nội dung và tiếp xúc trực tiếp |
| Quán ngại tự giao dù có gom đơn | Kiểm chứng sớm bằng câu hỏi "có 5 đơn gom sẵn thì có giao không?", nếu không thì chuyển sang pickup |
| Logistics phình to (lớp C) | Chỉ mở sau khi lớp B chạy ổn, trả shipper theo đơn, giới hạn một khu và một khung giờ, kiểm tra lợi nhuận mỗi đơn trước khi mở rộng |
| App lớn phản ứng | Tập trung vào cộng đồng địa phương và công cụ cho quán, tránh cạnh tranh trực diện về công nghệ |
| Nội dung so sánh giá gây rắc rối | Dùng số liệu thật, không công kích thương hiệu, chỉ đăng quán khi có sự đồng ý |

### Bước đầu tiên nên làm

Đi gặp và phỏng vấn 20 chủ quán trước khi viết bất kỳ dòng code nào. Ba câu hỏi quan trọng nhất:

1. "Hiện anh/chị mất bao nhiêu cho app giao hàng?"
2. "Nếu có 5 đơn gom sẵn cho một khu, anh/chị có giao không?"
3. "Anh/chị có chịu trả phí cố định hằng tháng cho công cụ quản lý đơn và khách quen không, mức nào thì hợp lý?"

---

## Phụ lục A: Kế hoạch nội dung để có natural traffic

**Nguyên tắc:** chọn một góc nhìn cụ thể ("người làm app giúp quán nhỏ không bị app lớn ăn hoa hồng"), build in public (quay hành trình thật, kể cả thất bại và số liệu), gắn với địa phương.

**4 trụ nội dung:**

| Trụ | Mục đích | Ví dụ |
|---|---|---|
| Nỗi đau quán nhỏ | Thu hút chủ quán | "Quán này bán 100k, app lấy bao nhiêu?" |
| Câu chuyện quán | Thu hút khách, tạo cảm xúc | Giới thiệu một quán, món nổi bật, chủ quán là ai |
| Build in public | Tạo niềm tin, thu hút cộng đồng khởi nghiệp | "Ngày thứ 7: gặp 10 quán, 3 quán đồng ý thử" |
| Mẹo hữu ích | Giữ người xem | "3 cách giúp quán nhỏ có thêm đơn không cần chạy quảng cáo" |

**Ý tưởng video (quay bằng điện thoại):**

1. "Tôi đi hỏi 10 chủ quán: app giao đồ ăn lấy bao nhiêu % của bạn?"
2. "Cùng một tô phở, khách trả bao nhiêu ở quán và trên app?"
3. "Mình muốn làm app cho quán nhỏ, đây là ý tưởng, mọi người góp ý giúp"
4. Một ngày đi giao thử cùng quán (trải nghiệm gom đơn theo khung giờ)
5. "Tôi thử bán giúp một quán trong 7 ngày, đây là kết quả"
6. Trước/sau: quán bán qua Zalo lộn xộn vs có trang đặt hàng gọn gàng
7. Series "Quán nhỏ có gì hay": mỗi tập 30–60 giây giới thiệu một quán

**Phân vai nền tảng:**

- **TikTok / Instagram Reels:** video ngắn 20–60 giây, mở đầu bằng câu hỏi hoặc con số gây tò mò trong 3 giây đầu; kênh chính để có lượt xem ngoài tệp quen.
- **Threads:** suy nghĩ, con số, bài học hằng ngày dạng chữ ngắn.
- **Facebook:** đăng vào nhóm khu phố, nhóm quán ăn, nhóm chủ shop nhỏ (tuân thủ nội quy nhóm) và Reels.
- **Instagram (feed/story):** hình ảnh quán, câu chuyện, hồ sơ thương hiệu.
- Quay một lần, đăng nhiều nơi.

**Kế hoạch 30 ngày:**

- **Tuần 1:** Định danh kênh, quay 5–7 video ngắn, đăng 1 video/ngày trên TikTok/Reels/Facebook.
- **Tuần 2:** Tiếp tục, thêm Threads hằng ngày; ghim bài "đang tìm 20 quán thử nghiệm" kèm cách liên hệ.
- **Tuần 3:** Chạy thử với các quán đã đăng ký, quay quá trình gom đơn/giao thử.
- **Tuần 4:** Đăng kết quả thật (số đơn, phản hồi), rút bài học, mời khách/quán vào nhóm cộng đồng.

**Lưu ý:** so sánh giá phải dùng số thật, không công kích thương hiệu; chỉ đăng quán khi được chủ quán đồng ý; mục tiêu là quán và khách thật, không chỉ lượt xem.

---

## Phụ lục B: Tiền lệ tham khảo

| Ví dụ | Điều đáng học | Lưu ý |
|---|---|---|
| **ChowNow (Mỹ)** | Thu phí thuê bao cố định thay vì hoa hồng; bán giao hàng theo từng đơn | Quán thường vẫn giữ app lớn để tìm khách mới, dùng ChowNow để giữ khách quen |
| **ONDC / magicpin (Ấn Độ)** | Tách các khâu (bán, mua, giao) để hoa hồng thấp; magicpin đạt 150.000 đơn/ngày trên mạng lưới | Tăng trưởng có trợ giá mạnh; nhiều số liệu đến từ nguồn quảng bá |
| **Dunzo (Ấn Độ)** | Bài học cảnh báo: mở rộng logistics quá sớm khi lợi nhuận mỗi đơn chưa vững dẫn đến đóng cửa vào 1/2025 | Chứng minh có lãi ở một khu nhỏ trước khi nhân rộng |

**Bài học chung về rò rỉ (platform leakage):** không có cách nào triệt để. Các nền tảng lớn giữ người dùng bằng niềm tin (đánh giá, bảo đảm, thanh toán an toàn), sự tiện lợi (chat, đặt lại nhanh) và kiểm soát khâu dịch vụ. Với mô hình này, hướng đúng là **thiết kế để nền tảng vẫn có giá trị dù khách có nhắn thẳng quán**.

> Lưu ý về nguồn: phần lớn số liệu về ChowNow và ONDC đến từ website công ty và blog đối tác, nên chỉ nên xem là tín hiệu thị trường. Các đối thủ trực tiếp tại thị trường của bạn chưa được khảo sát.
