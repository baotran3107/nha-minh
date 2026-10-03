# Backlog tính năng: Nền tảng kết nối quán địa phương (Backend trên Google Cloud)

Team: 1 BE + 1 FE. Quy ước: `[BE]` cho backend, `[FE]` cho frontend. Mỗi EPIC gắn nhãn giai đoạn theo lộ trình trong tài liệu ý tưởng.

| Nhãn | Ý nghĩa |
|---|---|
| **MVP** | Lớp A (quán tự giao / khách tự lấy), đủ để chạy thử 1 khu với 20-30 quán |
| **GĐ2** | Gom đơn theo khung giờ, công cụ giữ khách, thu phí thuê bao thử nghiệm |
| **GĐ3** | Thanh toán trong app, shipper khu vực, nhân rộng khu mới |

Thứ tự làm đề xuất: EPIC 0 → 9 cùng EPIC 15, 16 (MVP), sau đó EPIC 10 → 14 (GĐ2), cuối cùng EPIC 17, 18 (GĐ3). EPIC 19 (phi chức năng) làm xuyên suốt.

---

## Kiến trúc GCP đề xuất

| Nhu cầu | Dịch vụ GCP | Ghi chú |
|---|---|---|
| Chạy API | **Cloud Run** | Container, tự scale, có thể scale về 0 nên rẻ ở giai đoạn đầu |
| Database chính | **Cloud SQL for PostgreSQL** (+ PostGIS) | PostGIS dùng cho tìm quán theo khoảng cách. Tính tiền liên tục kể cả khi ít traffic, nên chọn cấu hình nhỏ lúc đầu |
| Xác thực | **Firebase Authentication / Identity Platform** | Chỉ đăng nhập Google. Zalo đi qua custom token do BE tự cấp. Không có đăng ký riêng, không dùng OTP/email |
| Realtime đơn hàng | **Firestore** (bản sao trạng thái đơn) | Postgres vẫn là nguồn dữ liệu chính. FE nghe Firestore để nhận đơn/trạng thái realtime, tránh phải giữ WebSocket trên Cloud Run |
| Push notification | **Firebase Cloud Messaging (FCM)** | |
| Hàng đợi / sự kiện | **Pub/Sub** | Sự kiện đơn hàng, thông báo, analytics |
| Tác vụ trễ / có retry | **Cloud Tasks** | Ví dụ tự hủy đơn khi quán không phản hồi, retry webhook |
| Job định kỳ | **Cloud Scheduler** + **Cloud Run Jobs** | Nhắc khách đặt lại, tính phí thuê bao, chốt khung giờ giao |
| Lưu ảnh/file | **Cloud Storage** | Upload bằng signed URL, thêm Cloud CDN khi lượng truy cập tăng |
| Bí mật/cấu hình | **Secret Manager** | Key Zalo, cổng thanh toán, key SMS/ZNS |
| CI/CD | **Cloud Build** + **Artifact Registry** | Build, test, đẩy image, deploy Cloud Run |
| Log/giám sát | **Cloud Logging, Error Reporting, Monitoring** | |
| Bản đồ | **Google Maps Platform** (Geocoding, Routes) | Tính phí theo lượt gọi, cần cache kết quả và đặt ngân sách |
| Phân tích | **BigQuery** (+ Looker Studio) | Chỉ số rò rỉ, chỉ số vận hành |
| Chống bot | **Firebase App Check / reCAPTCHA Enterprise** | Chống tạo đơn ảo, lạm dụng API |

Lưu ý chi phí: Cloud SQL, Google Maps API và ZNS/SMS (nếu dùng cho thông báo) có thể phát sinh phí. Vì chỉ đăng nhập bằng Google/Zalo nên không tốn phí OTP. Mức giá hiện hành nên xem trên trang pricing của từng dịch vụ và đặt budget alert ngay từ đầu (có task ở EPIC 0).

Các task FE có thay đổi nhỏ so với bản trước để khớp với kiến trúc này: đăng nhập qua Firebase Auth SDK, chỉ có Google và Zalo (EPIC 1), nhận realtime qua Firestore (EPIC 6, 7), nhận push qua FCM (EPIC 8).

---

## EPIC 0: Khởi tạo dự án & hạ tầng `MVP`

* Hạ tầng GCP
   * [BE] Tạo các GCP project tách riêng cho dev / staging / production, bật billing
   * [BE] Đặt Cloud Billing budget và alert chi phí cho từng project
   * [BE] Thiết lập IAM: service account riêng cho từng service, quyền tối thiểu
   * [BE] Bật các API cần dùng (Cloud Run, Cloud SQL, Secret Manager, Pub/Sub, Cloud Tasks, Cloud Scheduler...)
   * [BE] Tạo Firebase project gắn với GCP project (Auth, Firestore, FCM)
   * [BE] Setup Cloud SQL PostgreSQL, bật PostGIS, kết nối qua Cloud SQL connector/private IP
   * [BE] Bật backup tự động và point-in-time recovery cho Cloud SQL
   * [BE] Setup Secret Manager và quy ước đặt tên secret theo môi trường
   * [BE] Setup Cloud Storage bucket (ảnh công khai, file riêng tư), CORS và lifecycle rule
   * [BE] Setup tên miền, HTTPS cho Cloud Run (domain mapping hoặc load balancer)
   * [BE] (Tùy chọn) Viết Terraform cho hạ tầng chính để tái tạo được dev/staging/prod
* Dự án backend
   * [BE] Khởi tạo repo, cấu trúc project, chuẩn code (lint, format), Dockerfile
   * [BE] Setup Artifact Registry
   * [BE] Setup Cloud Build trigger: build → test → push image → deploy Cloud Run (dev/staging/production)
   * [BE] Cấu hình Cloud Run: min/max instances, concurrency, timeout, biến môi trường, kết nối Cloud SQL
   * [BE] Setup migration database và chạy migration qua Cloud Build/Cloud Run Job
   * [BE] Setup structured logging lên Cloud Logging, Error Reporting
   * [BE] Setup Cloud Monitoring: dashboard cơ bản và alert (lỗi 5xx, độ trễ, CPU/kết nối DB)
   * [BE] API health check và quy ước response/lỗi chuẩn
   * [BE] API lấy signed URL upload ảnh lên Cloud Storage
   * [BE] Resize/tối ưu ảnh sau khi upload (Cloud Storage event → Eventarc → Cloud Run)
   * [BE] Viết tài liệu API (OpenAPI/Swagger)
* Frontend
   * [FE] Khởi tạo repo FE (web responsive, ưu tiên mobile-first), cấu trúc project
   * [FE] Setup design system cơ bản (màu, typography, component dùng chung: button, input, card, modal)
   * [FE] Setup routing, state management, API client (xử lý token và lỗi chung)
   * [FE] Setup CI/CD và deploy FE (staging/production)
   * [FE] Setup đa môi trường (env config), cấu hình Firebase SDK theo môi trường

## EPIC 1: Đăng nhập (Google / Zalo) & Phân quyền `MVP`

Không có luồng đăng ký riêng: người dùng bắt buộc đăng nhập bằng Google hoặc Zalo, hồ sơ được tạo tự động ở lần đăng nhập đầu tiên.

* Nền tảng xác thực
   * [BE] Bật Firebase Authentication / Identity Platform chỉ với provider Google; tắt email/mật khẩu, số điện thoại, đăng nhập ẩn danh
   * [BE] Thiết kế database-schema cho users, auth_identities (provider: google/zalo, provider_uid, Firebase UID), roles, devices
   * [BE] Middleware xác thực: verify Firebase ID token bằng Admin SDK trên mỗi request
   * [BE] Phân quyền theo role (khách, chủ quán, admin) bằng custom claims và middleware kiểm tra role
   * [BE] Phân loại API: công khai (xem danh sách quán, menu) và API bắt buộc đăng nhập (đặt đơn, đánh giá, tạo quán, hồ sơ)
   * [BE] API đồng bộ người dùng sau đăng nhập: lần đầu tự tạo tài khoản (tên, avatar, email nếu có), các lần sau lấy lại tài khoản cũ
   * [BE] Chống lạm dụng: bật Firebase App Check/reCAPTCHA, rate limit theo user và IP cho đặt đơn/đánh giá, giới hạn số đơn đang chờ xử lý trên mỗi tài khoản
   * [BE] Ghi nhận ngày tạo tài khoản và áp giới hạn chặt hơn cho tài khoản mới
   * [BE] API khóa/mở khóa tài khoản vi phạm (dùng ở EPIC 15)
   * [BE] API thu hồi phiên/đăng xuất khỏi mọi thiết bị (revoke refresh tokens)
* Đăng nhập Google
   * [BE] Cấu hình OAuth client Google trong Firebase/Google Cloud (domain được phép, redirect URI)
   * [FE] Nút "Đăng nhập bằng Google" dùng Firebase Auth SDK
   * [FE] Gửi ID token lên API đồng bộ người dùng và xử lý kết quả
* Đăng nhập Zalo
   * [BE] Đăng ký ứng dụng trên Zalo for Developers, lưu app secret vào Secret Manager
   * [BE] API đăng nhập Zalo: nhận OAuth code, đổi access token (có PKCE), lấy thông tin người dùng
   * [BE] Tạo Firebase custom token từ tài khoản Zalo và trả về cho FE
   * [BE] (Tùy chọn) Liên kết với Zalo OA của nền tảng để gửi tin cho người dùng đã đăng nhập bằng Zalo, kiểm tra điều kiện của Zalo trước khi làm
   * [FE] Nút "Đăng nhập bằng Zalo", luồng redirect OAuth
   * [FE] Nhận custom token và đăng nhập bằng `signInWithCustomToken`
* Màn hình đăng nhập và phiên
   * [FE] Màn hình đăng nhập duy nhất với hai nút (Google, Zalo), không có form đăng ký
   * [FE] Tự chuyển sang màn hình đăng nhập khi người dùng chưa đăng nhập mà đặt hàng/đánh giá/tạo quán, đăng nhập xong quay lại đúng bước đang làm
   * [FE] Xử lý lỗi đăng nhập (người dùng hủy, Zalo/Google từ chối quyền, mất mạng)
   * [FE] Tự refresh token, xử lý hết phiên, đăng xuất
   * [FE] Route guard theo role
* Liên kết tài khoản
   * [BE] API liên kết thêm phương thức đăng nhập (Google ↔ Zalo) trong hồ sơ, tránh một người có hai tài khoản
   * [FE] Màn hình quản lý phương thức đăng nhập đã liên kết trong hồ sơ và integrate API
* Hồ sơ cá nhân khách
   * [BE] API xem/cập nhật hồ sơ (tên, avatar qua signed URL, số điện thoại liên hệ)
   * [BE] API quản lý sổ địa chỉ giao hàng (thêm/sửa/xóa/đặt mặc định)
   * [FE] Màn hình hồ sơ cá nhân và integrate API
   * [FE] Nhập số điện thoại liên hệ khi đặt đơn lần đầu (không xác minh OTP) và lưu vào hồ sơ
   * [FE] Màn hình quản lý địa chỉ giao hàng và integrate API

## EPIC 2: Khu vực & Onboarding quán `MVP`

* Quản lý khu vực (phường/chợ/khu dân cư)
   * [BE] Thiết kế database-schema cho khu vực (areas), lưu ranh giới bằng PostGIS, liên kết quán - khu vực
   * [BE] API danh sách khu vực
   * [BE] API xác định khu vực của khách (theo địa chỉ/GPS, dùng truy vấn PostGIS)
   * [BE] Tích hợp Google Maps Geocoding API để đổi địa chỉ ↔ tọa độ, cache kết quả vào DB để giảm số lần gọi
   * [FE] Màn hình/popup chọn khu vực
   * [FE] Integrate API khu vực, lưu khu vực đã chọn
* Đăng ký quán
   * [BE] Thiết kế database-schema cho shops (thông tin quán, loại hình, tọa độ, giờ mở cửa, trạng thái duyệt)
   * [BE] API tạo hồ sơ quán (nâng role chủ quán cho tài khoản đang đăng nhập)
   * [BE] API cập nhật hồ sơ quán (tên, mô tả, ảnh, địa chỉ, số liên hệ, giờ mở cửa)
   * [BE] API cấu hình giao hàng: bật/tắt giao, bán kính giao, phí ship, đơn tối thiểu, bật/tắt pickup
   * [BE] API trạng thái quán (đang mở / tạm nghỉ)
   * [BE] Gửi thông báo cho admin khi có quán mới chờ duyệt (Pub/Sub → email/Chat webhook)
   * [FE] Màn hình tạo hồ sơ quán (wizard nhiều bước, yêu cầu đã đăng nhập)
   * [FE] Màn hình hồ sơ quán và integrate API
   * [FE] Màn hình cấu hình giao hàng (bán kính, phí ship, pickup) và integrate API
   * [FE] Nút bật/tắt trạng thái mở cửa nhanh

## EPIC 3: Quản lý menu / sản phẩm `MVP`

* Danh mục & món
   * [BE] Thiết kế database-schema cho categories, products, option/topping, tồn kho cơ bản
   * [BE] API CRUD danh mục
   * [BE] API CRUD món/sản phẩm (tên, giá, mô tả, ảnh lưu trên Cloud Storage, trạng thái)
   * [BE] API bật/tắt món (hết hàng)
   * [BE] API CRUD option/topping (size, thêm món kèm)
   * [BE] API sắp xếp thứ tự danh mục/món
   * [BE] API import menu hàng loạt: upload file CSV/Excel lên Cloud Storage → Cloud Run Job xử lý → trả báo cáo lỗi từng dòng
   * [FE] Màn hình danh sách menu cho quán
   * [FE] Màn hình thêm/sửa món (kèm upload ảnh) và integrate API
   * [FE] Màn hình quản lý danh mục và integrate API
   * [FE] Toggle bật/tắt món nhanh
   * [FE] Kéo thả sắp xếp món/danh mục
   * [FE] Màn hình import menu và integrate API
* Khung giờ bán
   * [BE] API cấu hình món theo khung giờ (ví dụ chỉ bán buổi sáng)
   * [FE] UI cấu hình khung giờ bán

## EPIC 4: Khám phá & tìm quán gần khách `MVP`

* Danh sách quán trong khu vực
   * [BE] API danh sách quán theo khu vực/khoảng cách, có phân trang (PostGIS `ST_DWithin`)
   * [BE] Tạo GiST index cho cột vị trí và tối ưu truy vấn
   * [BE] API lọc/sắp xếp (đang mở, loại hình, đánh giá, có giao/pickup)
   * [BE] API tìm kiếm quán/món theo từ khóa, hỗ trợ tiếng Việt không dấu (Postgres `unaccent` + `pg_trgm`)
   * [BE] Cache danh sách quán/menu (HTTP cache hoặc Memorystore for Redis khi cần)
   * [FE] Màn hình trang chủ (danh sách quán gần khách)
   * [FE] Bộ lọc và sắp xếp, integrate API
   * [FE] Thanh tìm kiếm, gợi ý từ khóa, integrate API
   * [FE] Skeleton loading, trạng thái rỗng, infinite scroll
* Trang chi tiết quán
   * [BE] API chi tiết quán (thông tin, menu, giờ mở, bán kính giao, phí ship)
   * [FE] Màn hình chi tiết quán (header, menu theo danh mục, thông tin giao hàng)
   * [FE] Màn hình chi tiết món (chọn option/topping, ghi chú)
   * [FE] Nút liên hệ trực tiếp quán (Zalo/gọi) và nút chia sẻ link quán
   * [FE] SEO cơ bản và meta/OG tag cho trang quán (link chia sẻ đẹp trên Zalo/Facebook)

## EPIC 5: Giỏ hàng & Đặt hàng `MVP`

* Giỏ hàng
   * [BE] Thiết kế database-schema cho carts, cart_items
   * [BE] API thêm/sửa/xóa món trong giỏ, xem giỏ
   * [BE] Validate giỏ (món hết hàng, quán đóng cửa, khác quán, vượt bán kính)
   * [FE] Giỏ hàng (drawer/màn hình) và integrate API
   * [FE] Cập nhật số lượng, xóa món, ghi chú
   * [FE] Xử lý giỏ hàng khi chưa đăng nhập, gộp giỏ sau khi đăng nhập
* Checkout
   * [BE] Thiết kế database-schema cho orders, order_items, order_status_logs
   * [BE] API tính phí ship theo khoảng cách PostGIS/bậc khu vực (chỉ gọi Google Routes API khi thật sự cần quãng đường thực tế, có cache)
   * [BE] API tạo đơn (giao tận nơi hoặc pickup, thanh toán tiền mặt/chuyển khoản khi nhận), dùng transaction DB
   * [BE] Phát sự kiện `order.created` lên Pub/Sub sau khi tạo đơn
   * [BE] API áp dụng voucher (hook, hoàn thiện ở EPIC 10)
   * [BE] Chống tạo đơn trùng (idempotency key)
   * [FE] Màn hình checkout (địa chỉ, phương thức nhận, ghi chú, tổng tiền)
   * [FE] Integrate API tính phí ship và tạo đơn
   * [FE] Màn hình đặt hàng thành công

## EPIC 6: Quản lý đơn cho quán `MVP`

* Xử lý đơn
   * [BE] API danh sách đơn của quán (lọc theo trạng thái, ngày)
   * [BE] API xác nhận / từ chối đơn (kèm lý do)
   * [BE] API cập nhật trạng thái đơn (đang chuẩn bị, sẵn sàng, đang giao, hoàn thành, hủy), kiểm tra luồng chuyển trạng thái hợp lệ
   * [BE] Đồng bộ trạng thái đơn từ Postgres sang Firestore (Pub/Sub subscriber) để FE nhận realtime
   * [BE] Viết Firestore security rules: quán chỉ đọc đơn của mình, khách chỉ đọc đơn của mình
   * [BE] Tự hủy đơn khi quán không phản hồi quá thời gian quy định (Cloud Tasks hẹn giờ khi tạo đơn)
   * [FE] Màn hình danh sách đơn của quán (tab theo trạng thái)
   * [FE] Màn hình chi tiết đơn, nút đổi trạng thái, integrate API
   * [FE] Nhận đơn mới realtime bằng Firestore listener, âm thanh/rung cảnh báo
   * [FE] In/xem phiếu đơn (bản in đơn giản)
   * [FE] Màn hình đóng/mở bán, tạm nghỉ nhanh trong giờ cao điểm

## EPIC 7: Theo dõi đơn & Lịch sử (phía khách) `MVP`

* Theo dõi đơn
   * [BE] API chi tiết đơn và trạng thái của khách
   * [BE] API hủy đơn (theo điều kiện cho phép), đồng bộ lại sang Firestore
   * [FE] Màn hình theo dõi trạng thái đơn (realtime qua Firestore)
   * [FE] Nút hủy đơn và integrate API
* Lịch sử & đặt lại một chạm
   * [BE] API lịch sử đơn của khách (phân trang)
   * [BE] API đặt lại đơn cũ (tạo giỏ từ đơn cũ, kiểm tra món còn bán)
   * [FE] Màn hình lịch sử đơn
   * [FE] Nút "Đặt lại một chạm" và integrate API
   * [FE] Xử lý hiển thị khi món/quán không còn khả dụng

## EPIC 8: Thông báo `MVP`

* Hệ thống thông báo
   * [BE] Thiết kế database-schema cho notifications, thiết bị đăng ký push (FCM token)
   * [BE] API đăng ký/hủy FCM token của thiết bị
   * [BE] Worker thông báo: subscribe topic `order-events` trên Pub/Sub, gửi push qua FCM theo từng sự kiện đơn hàng
   * [BE] Tích hợp kênh ZNS/SMS (dịch vụ bên thứ ba, key lưu Secret Manager) cho thông báo quan trọng khi push không đến được
   * [BE] Retry và dead-letter queue cho thông báo thất bại (Pub/Sub dead-letter hoặc Cloud Tasks)
   * [BE] API danh sách thông báo, đánh dấu đã đọc
   * [BE] API cài đặt nhận thông báo
   * [FE] Đăng ký web push/push notification bằng FCM, gửi token lên API
   * [FE] Màn hình/Icon chuông thông báo trong app và integrate API
   * [FE] Màn hình cài đặt thông báo

## EPIC 9: Đánh giá & Xếp hạng `MVP`

* Đánh giá quán
   * [BE] Thiết kế database-schema cho reviews (chỉ khách đã hoàn thành đơn mới được đánh giá)
   * [BE] API tạo/sửa đánh giá (sao, nội dung, ảnh qua signed URL)
   * [BE] API danh sách đánh giá của quán và điểm trung bình (lưu sẵn điểm tổng hợp, cập nhật khi có đánh giá mới)
   * [BE] API quán phản hồi đánh giá
   * [BE] API báo cáo đánh giá vi phạm
   * [BE] Job tính xếp hạng quán theo khu vực (Cloud Scheduler → Cloud Run Job)
   * [FE] Màn hình/popup đánh giá sau khi hoàn thành đơn
   * [FE] Hiển thị điểm và danh sách đánh giá ở trang quán
   * [FE] Giao diện quán phản hồi đánh giá
   * [FE] Xếp hạng quán trong khu vực (hiển thị bảng xếp hạng)

## EPIC 10: Tích điểm, Voucher & Thẻ tích điểm `GĐ2`

* Tích điểm cho khách (chỉ dùng trong app)
   * [BE] Thiết kế database-schema cho points, point_transactions (ghi sổ cái, không sửa số dư trực tiếp)
   * [BE] Quy tắc tích điểm theo đơn hoàn thành (xử lý khi nhận sự kiện `order.completed` từ Pub/Sub)
   * [BE] API xem điểm và lịch sử điểm
   * [BE] API dùng điểm khi checkout
   * [FE] Màn hình ví điểm và lịch sử
   * [FE] Dùng điểm tại checkout và integrate API
* Voucher / ưu đãi
   * [BE] Thiết kế database-schema cho vouchers, voucher_usages
   * [BE] API quán tạo/sửa/tắt voucher (giảm %, giảm tiền, giới hạn lượt/thời gian)
   * [BE] API danh sách voucher của khách, kiểm tra điều kiện áp dụng
   * [BE] Hoàn thiện áp dụng voucher ở checkout, khóa dòng DB để tránh dùng vượt giới hạn lượt
   * [BE] Job hết hạn voucher (Cloud Scheduler)
   * [FE] Màn hình quán tạo/quản lý voucher
   * [FE] Màn hình ví voucher của khách
   * [FE] Chọn/nhập voucher tại checkout
* Thẻ tích điểm của quán
   * [BE] Thiết kế database-schema cho loyalty_cards (ví dụ mua 10 tặng 1)
   * [BE] API cấu hình thẻ tích điểm và tự đóng dấu theo đơn
   * [BE] API đóng dấu thủ công cho khách mua trực tiếp tại quán (QR/số điện thoại)
   * [FE] Màn hình cấu hình thẻ tích điểm cho quán
   * [FE] Màn hình thẻ tích điểm của khách theo từng quán
   * [FE] Màn hình quán quét QR/nhập số điện thoại để đóng dấu

## EPIC 11: Thống kê & Chăm sóc khách quen (công cụ cho quán) `GĐ2`

* Dashboard thống kê
   * [BE] API thống kê: doanh thu, số đơn, món bán chạy, khung giờ đông theo ngày/tuần/tháng (truy vấn Cloud SQL, dùng materialized view khi dữ liệu lớn)
   * [BE] API danh sách khách quen (tần suất mua, tổng chi tiêu, lần mua gần nhất)
   * [FE] Màn hình dashboard (biểu đồ doanh thu, đơn, món bán chạy, giờ đông)
   * [FE] Màn hình danh sách khách quen và chi tiết khách
* Chăm sóc khách
   * [BE] Job nhắc khách đặt lại theo chu kỳ mua (Cloud Scheduler → Cloud Run Job → Pub/Sub → FCM/ZNS)
   * [BE] API quán gửi ưu đãi cho nhóm khách (khách quen, lâu chưa quay lại)
   * [BE] Giới hạn tần suất gửi để tránh spam, tôn trọng cài đặt từ chối nhận thông báo
   * [FE] Màn hình tạo chiến dịch ưu đãi/nhắc khách
   * [FE] Màn hình xem kết quả chiến dịch (đã gửi, đã đặt)

## EPIC 12: Gói thuê bao & Quảng bá `GĐ2`

* Gói thuê bao
   * [BE] Thiết kế database-schema cho subscription_plans, shop_subscriptions, invoices
   * [BE] API danh sách gói, nâng/hạ gói, hủy gói
   * [BE] Phân quyền tính năng theo gói (feature flags lưu trong DB, kiểm tra ở middleware)
   * [BE] Job tính phí định kỳ, nhắc gia hạn, khóa tính năng khi hết hạn (Cloud Scheduler → Cloud Run Job)
   * [BE] Tích hợp cổng thanh toán Việt Nam (VNPay/MoMo/PayOS...) cho phí thuê bao, webhook chạy trên Cloud Run
   * [FE] Màn hình bảng giá và so sánh gói
   * [FE] Màn hình quản lý gói và hóa đơn của quán, integrate API
   * [FE] Hiển thị gợi ý nâng cấp khi quán chạm giới hạn tính năng
* Quảng bá quán
   * [BE] Thiết kế database-schema cho promotions (đẩy top, hiển thị ưu đãi, thời gian, khu vực)
   * [BE] API quán đăng ký/mua gói quảng bá
   * [BE] Thuật toán xếp hạng danh sách có tính vị trí quảng bá (gắn nhãn "Được tài trợ")
   * [BE] API báo cáo hiệu quả quảng bá (lượt xem, lượt đặt, lấy từ BigQuery hoặc bảng tổng hợp)
   * [FE] Màn hình đăng ký quảng bá cho quán
   * [FE] Hiển thị vị trí đẩy top và nhãn "Được tài trợ" trong danh sách
   * [FE] Màn hình báo cáo hiệu quả quảng bá

## EPIC 13: Gom đơn giao theo khung giờ (Lớp B) `GĐ2`

* Cấu hình khung giờ giao
   * [BE] Thiết kế database-schema cho delivery_slots, slot_orders
   * [BE] API quán cấu hình 1-3 khung giờ giao/ngày, ngưỡng đơn tối thiểu mỗi khung
   * [BE] API khách xem khung giờ giao khả dụng và chọn khi checkout
   * [BE] Logic gom đơn theo khu vực và khung giờ
   * [BE] Chốt khung giờ tự động: Cloud Tasks/Cloud Scheduler chạy trước giờ giao, kiểm tra ngưỡng đơn
   * [BE] Logic xử lý khi chưa đủ ngưỡng đơn (hủy slot, chuyển sang pickup, báo khách qua Pub/Sub → FCM)
   * [FE] Màn hình cấu hình khung giờ giao cho quán
   * [FE] Chọn khung giờ giao tại checkout và integrate API
   * [FE] Hiển thị tiến độ gom đơn ("còn 2 đơn nữa là chốt chuyến")
* Hỗ trợ quán giao một lượt
   * [BE] API danh sách đơn của một khung giờ, gom theo khu vực
   * [BE] Gợi ý lộ trình giao nhiều điểm bằng Google Routes API (tối ưu thứ tự điểm dừng), cache và giới hạn số lần gọi
   * [BE] Thông báo cho quán/khách khi chuyến được chốt
   * [FE] Màn hình "Chuyến giao" của quán (danh sách đơn gom sẵn)
   * [FE] Hiển thị lộ trình giao trên bản đồ và mở Google Maps
   * [FE] Xác nhận từng đơn đã giao trong chuyến

## EPIC 14: Đo lường & Chỉ số rò rỉ `GĐ2`

* Analytics sản phẩm
   * [BE] Thiết kế bảng sự kiện (events) và schema BigQuery
   * [BE] Pipeline thu thập: API nhận sự kiện từ FE → Pub/Sub → BigQuery (BigQuery subscription)
   * [BE] Đồng bộ dữ liệu đơn hàng từ Cloud SQL sang BigQuery (Datastream hoặc job export định kỳ)
   * [BE] Viết query/view chỉ số: quán hoạt động/tuần, đơn/quán, tỷ lệ quán trả phí
   * [BE] Chỉ số rò rỉ: tỷ lệ khách đặt lại qua nền tảng, số lượt bấm liên hệ trực tiếp (Zalo/gọi) so với số đơn
   * [BE] Chỉ số khung giờ: số đơn/khung giờ, chi phí giao trung bình/đơn
   * [BE] Chỉ số tăng trưởng: thời gian tuyển một quán mới, tỷ lệ chuyển đổi đăng ký
   * [BE] Dựng dashboard nội bộ trên Looker Studio (nối BigQuery), có thể thay một phần dashboard tự code
   * [FE] Gắn tracking sự kiện (xem quán, thêm giỏ, đặt đơn, bấm liên hệ trực tiếp)
   * [FE] (Tùy chọn) Tích hợp GA4/Firebase Analytics, export sang BigQuery
   * [FE] Màn hình dashboard chỉ số nội bộ (làm chung ở EPIC 15, hoặc nhúng Looker Studio)

## EPIC 15: Admin / Back-office `MVP` → `GĐ2`

* Quản trị
   * [BE] Cấp quyền admin bằng custom claims, có script tạo admin đầu tiên
   * [BE] API duyệt/từ chối/khóa quán
   * [BE] API quản lý người dùng, quán, đơn hàng (tra cứu, lọc)
   * [BE] API quản lý khu vực, danh mục quán, cấu hình hệ thống (phí ship bậc khu vực, ngưỡng đơn)
   * [BE] API xử lý khiếu nại, đánh giá bị báo cáo
   * [BE] Nhật ký thao tác admin (audit log trong DB và Cloud Logging)
   * [BE] (Tùy chọn) Đặt trang admin sau Identity-Aware Proxy để thêm một lớp bảo vệ
   * [FE] Màn hình đăng nhập admin
   * [FE] Màn hình duyệt quán và quản lý quán
   * [FE] Màn hình tra cứu đơn/người dùng
   * [FE] Màn hình quản lý khu vực và cấu hình hệ thống
   * [FE] Màn hình xử lý khiếu nại/báo cáo
   * [FE] Dashboard chỉ số nội bộ (hiển thị dữ liệu từ EPIC 14)

## EPIC 16: Trang giới thiệu & Tuyển quán (Go-to-market) `MVP`

* Landing page và thu hút quán
   * [BE] API nhận form đăng ký quan tâm của quán/khách (lead), chống spam bằng reCAPTCHA/App Check
   * [BE] API xuất danh sách lead cho admin (file CSV lưu Cloud Storage, link có thời hạn)
   * [BE] Thông báo lead mới cho team (Pub/Sub → email/Zalo/Chat)
   * [FE] Landing page giới thiệu cho chủ quán ("không ăn hoa hồng trên từng đơn")
   * [FE] Form "Đăng ký thử nghiệm" cho quán và integrate API
   * [FE] Landing page cho khách theo từng khu vực
   * [FE] Tối ưu SEO, tốc độ tải, chia sẻ mạng xã hội
   * [FE] Tạo mã QR/link giới thiệu riêng của quán để quán tự đưa khách vào nền tảng
* Giới thiệu & chia sẻ
   * [BE] API mã giới thiệu (referral) cho quán và khách
   * [FE] Màn hình chia sẻ link quán/mã giới thiệu

## EPIC 17: Thanh toán trong app `GĐ3`

* Thanh toán online
   * [BE] Thiết kế database-schema cho payments, refunds, payouts
   * [BE] Tích hợp cổng thanh toán (VNPay/MoMo/ZaloPay/PayOS), key lưu Secret Manager
   * [BE] Endpoint webhook trên Cloud Run: kiểm tra chữ ký, xử lý idempotent, trả về nhanh và đẩy việc nặng sang Pub/Sub/Cloud Tasks
   * [BE] Job đối soát giao dịch với cổng thanh toán (Cloud Scheduler → Cloud Run Job)
   * [BE] Đối soát và tất toán cho quán
   * [BE] API hoàn tiền khi đơn sai/trễ/hủy
   * [BE] Cơ chế giữ tiền tạm (escrow đơn giản) và tất toán cho quán sau khi đơn hoàn thành
   * [BE] API khiếu nại đơn và quy trình bảo vệ khách
   * [BE] Cảnh báo bất thường giao dịch (Cloud Monitoring alert trên log/metric)
   * [FE] Chọn phương thức thanh toán tại checkout
   * [FE] Màn hình thanh toán/redirect cổng và xử lý kết quả
   * [FE] Màn hình khiếu nại/yêu cầu hoàn tiền của khách
   * [FE] Màn hình đối soát/doanh thu và lịch tất toán cho quán

## EPIC 18: Shipper khu vực (Lớp C) `GĐ3`

* Quản lý shipper
   * [BE] Thiết kế database-schema cho shippers, shipper_zones, deliveries, shipper_payouts
   * [BE] API đăng ký/duyệt shipper tự do theo khu vực
   * [BE] Logic điều phối: gán đơn/chuyến cho shipper theo khu và khung giờ (Pub/Sub + Cloud Tasks cho timeout nhận chuyến)
   * [BE] Tính phí ship bậc khu vực/khung giờ và công trả shipper theo đơn
   * [BE] API shipper nhận chuyến, cập nhật trạng thái giao, upload ảnh xác nhận (signed URL Cloud Storage)
   * [BE] Cập nhật vị trí/trạng thái shipper realtime cho khách qua Firestore, kèm security rules
   * [BE] Báo cáo lợi nhuận mỗi đơn giao (BigQuery) để kiểm tra trước khi mở rộng
   * [FE] Màn hình đăng ký shipper
   * [FE] Màn hình shipper: danh sách chuyến, nhận chuyến, cập nhật trạng thái
   * [FE] Màn hình quán chọn "Thuê shipper khu vực" khi cấu hình giao hàng
   * [FE] Màn hình khách theo dõi shipper
   * [FE] Màn hình admin quản lý shipper và đối soát công shipper

## EPIC 19: Phi chức năng (Bảo mật, hiệu năng, vận hành) `Xuyên suốt`

* Bảo mật & tuân thủ
   * [BE] Rate limit ở tầng ứng dụng (và Cloud Armor nếu dùng load balancer), chống tạo đơn ảo và lạm dụng API
   * [BE] Rà soát IAM định kỳ, bật Cloud Audit Logs cho Cloud SQL, Secret Manager, Cloud Storage
   * [BE] Kiểm tra bucket Cloud Storage không public ngoài ý muốn, dùng signed URL cho file riêng tư
   * [BE] Quy trình backup/khôi phục Cloud SQL, diễn tập khôi phục ít nhất một lần
   * [BE] Quy trình xóa dữ liệu người dùng theo yêu cầu (xóa ở Firebase Auth, Cloud SQL, Firestore, Cloud Storage)
   * [BE] Chính sách quyền riêng tư và lưu trữ dữ liệu (thời hạn giữ log, dữ liệu đơn)
   * [FE] Màn hình điều khoản sử dụng, chính sách quyền riêng tư
   * [FE] Chặn XSS, validate input phía client, không lưu dữ liệu nhạy cảm ở local
* Chất lượng & hiệu năng
   * [BE] Unit test và integration test cho luồng đặt hàng, thanh toán, chạy trong Cloud Build
   * [BE] Test với Firebase Emulator/Cloud SQL local cho môi trường dev
   * [BE] Load test cho giờ cao điểm, tinh chỉnh autoscale và số kết nối DB của Cloud Run (connection pooling)
   * [BE] Theo dõi chi phí hằng tuần theo dịch vụ (Cloud SQL, Maps, SMS) và tối ưu
   * [BE] Thêm cache (Memorystore for Redis) khi chỉ số cho thấy cần
   * [FE] Test các luồng chính (E2E: đăng nhập, đặt hàng, quản lý đơn)
   * [FE] Tối ưu hiệu năng trên mạng yếu/điện thoại tầm thấp (lazy load, tối ưu ảnh)
   * [FE] Hỗ trợ PWA (cài lên màn hình chính, hoạt động mượt trên mobile)
   * [FE] Đa ngôn ngữ (nếu cần) và xử lý trạng thái lỗi/offline

---

## Gợi ý chia sprint cho 2 dev (tham khảo)

| Sprint | Mục tiêu | EPIC |
|---|---|---|
| 1 | Hạ tầng GCP + tài khoản | 0, 1 |
| 2 | Quán + menu | 2, 3 |
| 3 | Khách xem và đặt hàng | 4, 5 |
| 4 | Quán xử lý đơn + khách theo dõi + thông báo | 6, 7, 8 |
| 5 | Đánh giá + admin + landing để tuyển quán | 9, 15, 16 |
| 6+ | GĐ2: giữ khách, thuê bao, gom đơn, đo lường | 10, 11, 12, 13, 14 |
| Sau đó | GĐ3 khi đã có số liệu chứng minh | 17, 18 |

Ghi chú: Giai đoạn 1 trong tài liệu (kiểm chứng 1-2 tháng) có thể chạy thủ công qua Zalo/web đơn giản trong lúc team làm Sprint 1-3, nên chưa cần đủ toàn bộ EPIC MVP mới bắt đầu thử với quán.