# Backlog tính năng: Nền tảng kết nối quán địa phương

Team: 1 BE + 1 FE. Quy ước: `[BE]` cho backend, `[FE]` cho frontend. Mỗi EPIC gắn nhãn giai đoạn theo lộ trình trong tài liệu ý tưởng.

| Nhãn | Ý nghĩa |
|---|---|
| **MVP** | Lớp A (quán tự giao / khách tự lấy), đủ để chạy thử 1 khu với 20-30 quán |
| **GĐ2** | Gom đơn theo khung giờ, công cụ giữ khách, thu phí thuê bao thử nghiệm |
| **GĐ3** | Thanh toán trong app, shipper khu vực, nhân rộng khu mới |

Thứ tự làm đề xuất: EPIC 0 → 9 cùng EPIC 15, 16 (MVP), sau đó EPIC 10 → 14 (GĐ2), cuối cùng EPIC 17, 18 (GĐ3). EPIC 19 (phi chức năng) làm xuyên suốt.

---

## EPIC 0: Khởi tạo dự án & hạ tầng `MVP`

* Setup dự án
   * [BE] Khởi tạo repo, cấu trúc project, chuẩn code (lint, format)
   * [BE] Setup môi trường dev/staging/production
   * [BE] Setup database (PostgreSQL) và migration tool
   * [BE] Setup CI/CD cho BE
   * [BE] Setup logging, error tracking
   * [BE] Setup lưu trữ ảnh (object storage) và API upload ảnh
   * [BE] Viết tài liệu API (Swagger/OpenAPI)
   * [FE] Khởi tạo repo FE (web responsive, ưu tiên mobile-first), cấu trúc project
   * [FE] Setup design system cơ bản (màu, typography, component dùng chung: button, input, card, modal)
   * [FE] Setup routing, state management, API client (axios/fetch wrapper, xử lý token và lỗi chung)
   * [FE] Setup CI/CD và deploy FE (staging/production)
   * [FE] Setup đa môi trường (env config)

## EPIC 1: Đăng ký / Đăng nhập / Phân quyền `MVP`

* Tài khoản khách hàng
   * [BE] Thiết kế database-schema cho users, roles, sessions
   * [BE] API đăng ký (số điện thoại + OTP hoặc email)
   * [BE] API đăng nhập
   * [BE] API đăng nhập qua Zalo/Google (tùy chọn)
   * [BE] API refresh token, đăng xuất
   * [BE] API quên mật khẩu / đổi mật khẩu
   * [BE] Tích hợp dịch vụ gửi OTP (SMS/Zalo ZNS)
   * [BE] Middleware phân quyền theo role (khách, chủ quán, admin)
   * [FE] Màn hình đăng ký
   * [FE] Integrate API đăng ký
   * [FE] Màn hình đăng nhập
   * [FE] Integrate API đăng nhập
   * [FE] Màn hình nhập OTP
   * [FE] Màn hình quên/đổi mật khẩu và integrate API
   * [FE] Xử lý lưu token, tự refresh, tự đăng xuất khi hết hạn
   * [FE] Route guard theo role
* Hồ sơ cá nhân khách
   * [BE] API xem/cập nhật hồ sơ (tên, avatar)
   * [BE] API quản lý sổ địa chỉ giao hàng (thêm/sửa/xóa/đặt mặc định)
   * [FE] Màn hình hồ sơ cá nhân và integrate API
   * [FE] Màn hình quản lý địa chỉ giao hàng và integrate API

## EPIC 2: Khu vực & Onboarding quán `MVP`

* Quản lý khu vực (phường/chợ/khu dân cư)
   * [BE] Thiết kế database-schema cho khu vực (areas), liên kết quán - khu vực
   * [BE] API danh sách khu vực
   * [BE] API xác định khu vực của khách (theo địa chỉ/GPS)
   * [FE] Màn hình/popup chọn khu vực
   * [FE] Integrate API khu vực, lưu khu vực đã chọn
* Đăng ký quán
   * [BE] Thiết kế database-schema cho shops (thông tin quán, loại hình, giờ mở cửa, trạng thái duyệt)
   * [BE] API đăng ký quán (tạo tài khoản chủ quán + hồ sơ quán)
   * [BE] API cập nhật hồ sơ quán (tên, mô tả, ảnh, địa chỉ, số liên hệ, giờ mở cửa)
   * [BE] API cấu hình giao hàng: bật/tắt giao, bán kính giao, phí ship, đơn tối thiểu, bật/tắt pickup
   * [BE] API trạng thái quán (đang mở / tạm nghỉ)
   * [FE] Màn hình đăng ký quán (wizard nhiều bước)
   * [FE] Màn hình hồ sơ quán và integrate API
   * [FE] Màn hình cấu hình giao hàng (bán kính, phí ship, pickup) và integrate API
   * [FE] Nút bật/tắt trạng thái mở cửa nhanh

## EPIC 3: Quản lý menu / sản phẩm `MVP`

* Danh mục & món
   * [BE] Thiết kế database-schema cho categories, products, option/topping, tồn kho cơ bản
   * [BE] API CRUD danh mục
   * [BE] API CRUD món/sản phẩm (tên, giá, mô tả, ảnh, trạng thái)
   * [BE] API bật/tắt món (hết hàng)
   * [BE] API CRUD option/topping (size, thêm món kèm)
   * [BE] API sắp xếp thứ tự danh mục/món
   * [BE] API import menu hàng loạt (CSV/Excel) để giảm công onboarding
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
   * [BE] API danh sách quán theo khu vực/khoảng cách, có phân trang
   * [BE] API lọc/sắp xếp (đang mở, loại hình, đánh giá, có giao/pickup)
   * [BE] API tìm kiếm quán/món theo từ khóa
   * [BE] Tối ưu truy vấn theo vị trí (geo index)
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
   * [BE] API tính phí ship theo khoảng cách/bậc khu vực
   * [BE] API tạo đơn (chọn giao tận nơi hoặc pickup, thanh toán tiền mặt/chuyển khoản khi nhận)
   * [BE] API áp dụng voucher (hook, hoàn thiện ở EPIC 10)
   * [BE] Chống tạo đơn trùng (idempotency)
   * [FE] Màn hình checkout (địa chỉ, phương thức nhận, ghi chú, tổng tiền)
   * [FE] Integrate API tính phí ship và tạo đơn
   * [FE] Màn hình đặt hàng thành công

## EPIC 6: Quản lý đơn cho quán `MVP`

* Xử lý đơn
   * [BE] API danh sách đơn của quán (lọc theo trạng thái, ngày)
   * [BE] API xác nhận / từ chối đơn (kèm lý do)
   * [BE] API cập nhật trạng thái đơn (đang chuẩn bị, sẵn sàng, đang giao, hoàn thành, hủy)
   * [BE] Realtime đơn mới cho quán (WebSocket/SSE)
   * [BE] Tự hủy đơn khi quán không phản hồi quá thời gian quy định
   * [FE] Màn hình danh sách đơn của quán (tab theo trạng thái)
   * [FE] Màn hình chi tiết đơn, nút đổi trạng thái, integrate API
   * [FE] Nhận đơn mới realtime, âm thanh/rung cảnh báo
   * [FE] In/xem phiếu đơn (bản in đơn giản)
   * [FE] Màn hình đóng/mở bán, tạm nghỉ nhanh trong giờ cao điểm

## EPIC 7: Theo dõi đơn & Lịch sử (phía khách) `MVP`

* Theo dõi đơn
   * [BE] API chi tiết đơn và trạng thái của khách
   * [BE] API hủy đơn (theo điều kiện cho phép)
   * [BE] Realtime cập nhật trạng thái đơn cho khách
   * [FE] Màn hình theo dõi trạng thái đơn
   * [FE] Nút hủy đơn và integrate API
* Lịch sử & đặt lại một chạm
   * [BE] API lịch sử đơn của khách (phân trang)
   * [BE] API đặt lại đơn cũ (tạo giỏ từ đơn cũ, kiểm tra món còn bán)
   * [FE] Màn hình lịch sử đơn
   * [FE] Nút "Đặt lại một chạm" và integrate API
   * [FE] Xử lý hiển thị khi món/quán không còn khả dụng

## EPIC 8: Thông báo `MVP`

* Hệ thống thông báo
   * [BE] Thiết kế database-schema cho notifications, thiết bị đăng ký push
   * [BE] Service gửi thông báo (push/Zalo ZNS/SMS/email) theo sự kiện đơn hàng
   * [BE] API danh sách thông báo, đánh dấu đã đọc
   * [BE] API cài đặt nhận thông báo
   * [FE] Đăng ký web push / push notification
   * [FE] Màn hình/Icon chuông thông báo trong app và integrate API
   * [FE] Màn hình cài đặt thông báo

## EPIC 9: Đánh giá & Xếp hạng `MVP`

* Đánh giá quán
   * [BE] Thiết kế database-schema cho reviews (chỉ khách đã hoàn thành đơn mới được đánh giá)
   * [BE] API tạo/sửa đánh giá (sao, nội dung, ảnh)
   * [BE] API danh sách đánh giá của quán và điểm trung bình
   * [BE] API quán phản hồi đánh giá
   * [BE] API báo cáo đánh giá vi phạm
   * [FE] Màn hình/popup đánh giá sau khi hoàn thành đơn
   * [FE] Hiển thị điểm và danh sách đánh giá ở trang quán
   * [FE] Giao diện quán phản hồi đánh giá
   * [FE] Xếp hạng quán trong khu vực (hiển thị bảng xếp hạng)

## EPIC 10: Tích điểm, Voucher & Thẻ tích điểm `GĐ2`

* Tích điểm cho khách (chỉ dùng trong app)
   * [BE] Thiết kế database-schema cho points, point_transactions
   * [BE] Quy tắc tích điểm theo đơn hoàn thành
   * [BE] API xem điểm và lịch sử điểm
   * [BE] API dùng điểm khi checkout
   * [FE] Màn hình ví điểm và lịch sử
   * [FE] Dùng điểm tại checkout và integrate API
* Voucher / ưu đãi
   * [BE] Thiết kế database-schema cho vouchers, voucher_usages
   * [BE] API quán tạo/sửa/tắt voucher (giảm %, giảm tiền, giới hạn lượt/thời gian)
   * [BE] API danh sách voucher của khách, kiểm tra điều kiện áp dụng
   * [BE] Hoàn thiện áp dụng voucher ở checkout
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
   * [BE] API thống kê: doanh thu, số đơn, món bán chạy, khung giờ đông theo ngày/tuần/tháng
   * [BE] API danh sách khách quen (tần suất mua, tổng chi tiêu, lần mua gần nhất)
   * [FE] Màn hình dashboard (biểu đồ doanh thu, đơn, món bán chạy, giờ đông)
   * [FE] Màn hình danh sách khách quen và chi tiết khách
* Chăm sóc khách
   * [BE] Nhắc khách đặt lại (job định kỳ theo chu kỳ mua)
   * [BE] API quán gửi ưu đãi cho nhóm khách (khách quen, lâu chưa quay lại)
   * [BE] Giới hạn tần suất gửi để tránh spam
   * [FE] Màn hình tạo chiến dịch ưu đãi/nhắc khách
   * [FE] Màn hình xem kết quả chiến dịch (đã gửi, đã đặt)

## EPIC 12: Gói thuê bao & Quảng bá `GĐ2`

* Gói thuê bao
   * [BE] Thiết kế database-schema cho subscription_plans, shop_subscriptions, invoices
   * [BE] API danh sách gói, nâng/hạ gói, hủy gói
   * [BE] Phân quyền tính năng theo gói (feature flags: miễn phí / cơ bản / nâng cao)
   * [BE] Job tính phí định kỳ, nhắc gia hạn, khóa tính năng khi hết hạn
   * [BE] Tích hợp cổng thanh toán cho phí thuê bao (VNPay/MoMo/PayOS...)
   * [FE] Màn hình bảng giá và so sánh gói
   * [FE] Màn hình quản lý gói và hóa đơn của quán, integrate API
   * [FE] Hiển thị gợi ý nâng cấp khi quán chạm giới hạn tính năng
* Quảng bá quán
   * [BE] Thiết kế database-schema cho promotions (đẩy top, hiển thị ưu đãi, thời gian, khu vực)
   * [BE] API quán đăng ký/mua gói quảng bá
   * [BE] Thuật toán xếp hạng danh sách có tính vị trí quảng bá (gắn nhãn "Được tài trợ")
   * [BE] API báo cáo hiệu quả quảng bá (lượt xem, lượt đặt)
   * [FE] Màn hình đăng ký quảng bá cho quán
   * [FE] Hiển thị vị trí đẩy top và nhãn "Được tài trợ" trong danh sách
   * [FE] Màn hình báo cáo hiệu quả quảng bá

## EPIC 13: Gom đơn giao theo khung giờ (Lớp B) `GĐ2`

* Cấu hình khung giờ giao
   * [BE] Thiết kế database-schema cho delivery_slots, slot_orders
   * [BE] API quán cấu hình 1-3 khung giờ giao/ngày, ngưỡng đơn tối thiểu mỗi khung
   * [BE] API khách xem khung giờ giao khả dụng và chọn khi checkout
   * [BE] Logic gom đơn theo khu vực và khung giờ
   * [BE] Logic xử lý khi chưa đủ ngưỡng đơn (hủy slot, chuyển sang pickup, báo khách)
   * [FE] Màn hình cấu hình khung giờ giao cho quán
   * [FE] Chọn khung giờ giao tại checkout và integrate API
   * [FE] Hiển thị tiến độ gom đơn ("còn 2 đơn nữa là chốt chuyến")
* Hỗ trợ quán giao một lượt
   * [BE] API danh sách đơn của một khung giờ, gom theo khu vực
   * [BE] Gợi ý lộ trình giao (sắp xếp điểm giao tối ưu)
   * [BE] Job chốt khung giờ và gửi thông báo cho quán/khách
   * [FE] Màn hình "Chuyến giao" của quán (danh sách đơn gom sẵn)
   * [FE] Hiển thị lộ trình giao trên bản đồ và mở Google Maps
   * [FE] Xác nhận từng đơn đã giao trong chuyến

## EPIC 14: Đo lường & Chỉ số rò rỉ `GĐ2`

* Analytics sản phẩm
   * [BE] Thiết kế bảng sự kiện (events) và pipeline thu thập
   * [BE] API/Query chỉ số: quán hoạt động/tuần, đơn/quán, tỷ lệ quán trả phí
   * [BE] Chỉ số rò rỉ: tỷ lệ khách đặt lại qua nền tảng, số lượt bấm liên hệ trực tiếp (Zalo/gọi) so với số đơn
   * [BE] Chỉ số khung giờ: số đơn/khung giờ, chi phí giao trung bình/đơn
   * [BE] Chỉ số tăng trưởng: thời gian tuyển một quán mới, tỷ lệ chuyển đổi đăng ký
   * [FE] Gắn tracking sự kiện (xem quán, thêm giỏ, đặt đơn, bấm liên hệ trực tiếp)
   * [FE] Tích hợp công cụ analytics (GA4/PostHog/Mixpanel)
   * [FE] Màn hình dashboard chỉ số nội bộ (làm chung ở EPIC 15)

## EPIC 15: Admin / Back-office `MVP` → `GĐ2`

* Quản trị
   * [BE] API duyệt/từ chối/khóa quán
   * [BE] API quản lý người dùng, quán, đơn hàng (tra cứu, lọc)
   * [BE] API quản lý khu vực, danh mục quán, cấu hình hệ thống (phí ship bậc khu vực, ngưỡng đơn)
   * [BE] API xử lý khiếu nại, đánh giá bị báo cáo
   * [BE] Nhật ký thao tác admin (audit log)
   * [FE] Màn hình đăng nhập admin
   * [FE] Màn hình duyệt quán và quản lý quán
   * [FE] Màn hình tra cứu đơn/người dùng
   * [FE] Màn hình quản lý khu vực và cấu hình hệ thống
   * [FE] Màn hình xử lý khiếu nại/báo cáo
   * [FE] Dashboard chỉ số nội bộ (hiển thị dữ liệu từ EPIC 14)

## EPIC 16: Trang giới thiệu & Tuyển quán (Go-to-market) `MVP`

* Landing page và thu hút quán
   * [BE] API nhận form đăng ký quan tâm của quán/khách (lead)
   * [BE] API xuất danh sách lead cho admin
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
   * [BE] Tích hợp cổng thanh toán (VNPay/MoMo/ZaloPay/PayOS) và webhook xác nhận
   * [BE] Đối soát giao dịch và đối soát với quán
   * [BE] API hoàn tiền khi đơn sai/trễ/hủy
   * [BE] Cơ chế giữ tiền tạm (escrow đơn giản) và tất toán cho quán sau khi đơn hoàn thành
   * [BE] API khiếu nại đơn và quy trình bảo vệ khách
   * [FE] Chọn phương thức thanh toán tại checkout
   * [FE] Màn hình thanh toán/redirect cổng và xử lý kết quả
   * [FE] Màn hình khiếu nại/yêu cầu hoàn tiền của khách
   * [FE] Màn hình đối soát/doanh thu và lịch tất toán cho quán

## EPIC 18: Shipper khu vực (Lớp C) `GĐ3`

* Quản lý shipper
   * [BE] Thiết kế database-schema cho shippers, shipper_zones, deliveries, shipper_payouts
   * [BE] API đăng ký/duyệt shipper tự do theo khu vực
   * [BE] Logic điều phối: gán đơn/chuyến cho shipper theo khu và khung giờ
   * [BE] Tính phí ship bậc khu vực/khung giờ và công trả shipper theo đơn
   * [BE] API shipper nhận chuyến, cập nhật trạng thái giao, upload ảnh xác nhận
   * [BE] Realtime vị trí/trạng thái giao cho khách
   * [BE] Báo cáo lợi nhuận mỗi đơn giao (để kiểm tra trước khi mở rộng)
   * [FE] Màn hình đăng ký shipper
   * [FE] Màn hình shipper: danh sách chuyến, nhận chuyến, cập nhật trạng thái
   * [FE] Màn hình quán chọn "Thuê shipper khu vực" khi cấu hình giao hàng
   * [FE] Màn hình khách theo dõi shipper
   * [FE] Màn hình admin quản lý shipper và đối soát công shipper

## EPIC 19: Phi chức năng (Bảo mật, hiệu năng, vận hành) `Xuyên suốt`

* Bảo mật & tuân thủ
   * [BE] Rate limit, chống spam OTP và tạo đơn ảo
   * [BE] Mã hóa dữ liệu nhạy cảm, hash mật khẩu, quản lý secret
   * [BE] Backup database định kỳ và quy trình khôi phục
   * [BE] Chính sách quyền riêng tư, xóa dữ liệu theo yêu cầu người dùng
   * [FE] Màn hình điều khoản sử dụng, chính sách quyền riêng tư
   * [FE] Chặn XSS, validate input phía client, không lưu dữ liệu nhạy cảm ở local
* Chất lượng & hiệu năng
   * [BE] Unit test và integration test cho luồng đặt hàng, thanh toán
   * [BE] Load test cho giờ cao điểm
   * [BE] Cache cho danh sách quán/menu
   * [FE] Test các luồng chính (E2E: đăng nhập, đặt hàng, quản lý đơn)
   * [FE] Tối ưu hiệu năng trên mạng yếu/điện thoại tầm thấp (lazy load, tối ưu ảnh)
   * [FE] Hỗ trợ PWA (cài lên màn hình chính, hoạt động mượt trên mobile)
   * [FE] Đa ngôn ngữ (nếu cần) và xử lý trạng thái lỗi/offline

---

## Gợi ý chia sprint cho 2 dev (tham khảo)

| Sprint | Mục tiêu | EPIC |
|---|---|---|
| 1 | Nền tảng + tài khoản | 0, 1 |
| 2 | Quán + menu | 2, 3 |
| 3 | Khách xem và đặt hàng | 4, 5 |
| 4 | Quán xử lý đơn + khách theo dõi | 6, 7, 8 |
| 5 | Đánh giá + admin + landing để tuyển quán | 9, 15, 16 |
| 6+ | GĐ2: giữ khách, thuê bao, gom đơn, đo lường | 10, 11, 12, 13, 14 |
| Sau đó | GĐ3 khi đã có số liệu chứng minh | 17, 18 |

Ghi chú: Giai đoạn 1 trong tài liệu (kiểm chứng 1-2 tháng) có thể chạy thủ công qua Zalo/web đơn giản trong lúc team làm Sprint 1-3, nên chưa cần đủ toàn bộ EPIC MVP mới bắt đầu thử với quán.