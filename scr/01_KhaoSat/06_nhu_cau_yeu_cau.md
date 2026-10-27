# Bảng Nhu cầu → Yêu cầu (Needs → Requirements Traceability)

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

## 1. Ma trận truy vết Nhu cầu – Yêu cầu – Use Case

| STT | Nhu cầu (Stakeholder) | Nguồn (Phỏng vấn / Bảng hỏi / Benchmark) | Yêu cầu đối với hệ thống (FR/NFR/BR) | UC dự kiến | Mức ưu tiên |
|-----|------------------------|-------------------------------------------|--------------------------------------|------------|-------------|
| **KH-01** | Khách hàng duyệt sản phẩm, tìm kiếm/lọc theo danh mục, độ tuổi, giá, thương hiệu | Phỏng vấn chủ shop + Benchmark (Mykingdom, Con Cưng, Shopee) | **FR-01:** Hiển thị danh sách sản phẩm có phân trang, bộ lọc đa tiêu chí (danh mục, độ tuổi, giá, thương hiệu, độ an toàn)<br>**NFR-01:** Thời gian tải trang < 2s, hỗ trợ responsive trên mobile/desktop<br>**BR-01:** Sản phẩm hết hàng hiển thị badge "Hết hàng" nhưng vẫn cho xem chi tiết | UC-01: Xem danh sách sản phẩm<br>UC-02: Tìm kiếm & lọc sản phẩm<br>UC-03: Xem chi tiết sản phẩm | Cao |
| **KH-02** | Khách thêm sản phẩm vào giỏ hàng, chỉnh sửa số lượng, xóa món | Bảng hỏi phụ huynh (câu 3: ngại mua online) + Benchmark | **FR-02:** Giỏ hàng kéo (slide-out) / trang riêng, cập nhật real-time số lượng, thành tiền<br>**FR-03:** Giỏ hàng lưu localStorage (khách chưa đăng nhập) và đồng bộ khi đăng nhập<br>**BR-02:** Số lượng thêm vào giỏ ≤ tồn kho thực tế tại thời điểm thêm | UC-04: Quản lý giỏ hàng | Cao |
| **KH-03** | Khách đăng nhập/đăng ký tài khoản để đặt hàng, xem lịch sử | Phỏng vấn chủ shop (câu 1, 5) + Bảng hỏi (câu 5) | **FR-04:** Đăng ký bằng email/số điện thoại, xác thực OTP/email<br>**FR-05:** Đăng nhập, quên mật khẩu, đổi mật khẩu<br>**NFR-02:** Mật khẩu hash bcrypt, session JWT có hết hạn<br>**BR-03:** 1 email/sđt chỉ đăng ký 1 tài khoản | UC-05: Đăng ký tài khoản<br>UC-06: Đăng nhập/Đăng xuất<br>UC-07: Quản lý hồ sơ cá nhân | Cao |
| **KH-04** | Khách đặt hàng: nhập địa chỉ, chọn phương thức giao, chọn thanh toán (COD/Chuyển khoản/Cổng mô phỏng), áp dụng coupon | Quy trình To-Be + Bảng hỏi (câu 4) | **FR-06:** Form checkout 1 trang: địa chỉ (tự động gợi ý tỉnh/huyện/xã), phương thức giao, thanh toán, coupon<br>**FR-07:** Kiểm tra tồn kho real-time khi bấm "Đặt hàng"<br>**FR-08:** Validate coupon (mã, hạn dùng, điều kiện đơn tối thiểu, giới hạn dùng/user)<br>**BR-04:** Miễn phí ship khi đơn ≥ 500.000đ (cần chủ shop xác nhận)<br>**BR-05:** Phí ship tính theo khu vực + khối lượng (cần chủ shop xác nhận) | UC-08: Thanh toán & đặt hàng | Cao |
| **KH-05** | Khách theo dõi trạng thái đơn hàng trên website (mã vận đơn, vị trí giao, lịch sử) | Bảng hỏi (câu 5) + Benchmark | **FR-09:** Trang "Đơn hàng của tôi" liệt kê đơn với trạng thái: Chờ xác nhận → Đang đóng gói → Đang giao → Hoàn tất / Hủy<br>**FR-10:** Chi tiết đơn: timeline trạng thái, mã vận đơn (link truy cập hãng ship mô phỏng), thông tin người nhận, phí ship, coupon<br>**NFR-03:** Cập nhật trạng thái từ hãng ship mô phỏng near real-time (< 30s) | UC-09: Tra cứu & theo dõi đơn hàng | Cao |
| **KH-06** | Khách muốn thanh toán an toàn, đa dạng phương thức | Bảng hỏi (câu 4) + Phạm vi đề tài | **FR-11:** Hỗ trợ 3 phương thức: COD, Chuyển khoản (hiển thị thông tin ngân hàng + nội dung chuyển), Cổng thanh toán mô phỏng (trả về success/fail ngẫu nhiên theo config)<br>**NFR-04:** Không lưu thông tin thẻ thật, prototype chỉ mô phỏng luồng | UC-10: Thanh toán đơn hàng | Cao |
| **NV-01** | Nhân viên xác nhận đơn: kiểm tra tồn kho, địa chỉ, coupon, phí ship → xác nhận/hủy | Quy trình To-Be + Phỏng vấn chủ shop (câu 2, 3) | **FR-12:** Danh sách đơn "Chờ xác nhận" với bộ lọc, tìm kiếm<br>**FR-13:** Modal chi tiết đơn: hiển thị tồn kho từng SKU, validate địa chỉ (tự động check khu vực phục vụ), hiển thị coupon áp dụng, phí ship tính toán<br>**BR-06:** Nhân viên có quyền ghi đè phí ship / coupon trong trường hợp ngoại lệ (ghi log lý do)<br>**BR-07:** Nếu tồn kho không đủ → chuyển trạng thái "Chờ bổ sung hàng" để chủ shop xử lý | UC-11: Xác nhận đơn hàng | Cao |
| **NV-02** | Nhân viên đóng gói, tạo vận đơn mô phỏng (mã vận đơn, hãng ship, trọng lượng) | Quy trình To-Be + Phỏng vấn (câu 3) | **FR-14:** Màn hình "Đóng gói": check từng dòng sản phẩm, in phiếu đóng gói (PDF)<br>**FR-15:** Tạo vận đơn: nhập/hiển thị hãng ship (GHN/GHTK/J&T mô phỏng), tự động sinh mã vận đơn theo format, tính trọng lượng từ quy cách sản phẩm<br>**BR-08:** Mã vận đơn format: `WH-YYYYMMDD-XXXX` (XXXX tự tăng)<br>**FR-16:** Sau khi tạo vận đơn → tự động chuyển trạng thái "Đang giao", gửi thông báo cho khách (email/UI) | UC-12: Đóng gói & tạo vận đơn | Cao |
| **NV-03** | Nhân viên xử lý ngoại lệ: thanh toán lỗi, giao hàng thất bại, khách hủy | Quy trình To-Be (bước lỗi) | **FR-17:** Trang "Đơn cần xử lý": lọc theo loại lỗi (Thanh toán lỗi / Giao thất bại / Khách hủy)<br>**FR-18:** Hành động: Thử lại thanh toán, Liên hệ khách đổi địa chỉ/sđt, Hủy đơn hoàn tồn kho, Ghi chú nội bộ<br>**BR-09:** Mọi thao tác ngoại lệ ghi log: user, thời gian, hành động, lý do<br>**NFR-05:** Audit log không thể xóa, chỉ append | UC-13: Xử lý ngoại lệ đơn hàng | Cao |
| **CS-01** | Chủ cửa hàng/Quản trị: quản lý danh mục, sản phẩm, tồn kho, hình ảnh | Phỏng vấn chủ shop (câu 2, 4, 5) | **FR-19:** CRUD Danh mục (cấp 2), Sản phẩm (tên, mô tả, hình ảnh multiple, giá, giá sale, SKU, thuộc tính: độ tuổi, thương hiệu, trọng lượng, kích thước)<br>**FR-20:** Quản lý tồn kho: nhập/xuất kho, điều chỉnh, cảnh báo tồn kho thấp (ngưỡng cấu hình), lịch sử nhập xuất<br>**BR-10:** SKU tự động sinh hoặc nhập tay, duy nhất<br>**NFR-06:** Upload ảnh lên local storage / cloud (minio/S3), resize tự động thumbnail | UC-14: Quản lý danh mục & sản phẩm<br>UC-15: Quản lý tồn kho | Cao |
| **CS-02** | Chủ cửa hàng: quản lý coupon, quy tắc phí ship, ngưỡng miễn phí | Phạm vi + Giả định 1.5 | **FR-21:** CRUD Coupon: mã, loại (% / cố định), giá trị, điều kiện (đơn tối thiểu, sản phẩm/danh mục áp dụng), giới hạn tổng/người dùng, ngày bắt đầu/kết thúc, trạng thái<br>**FR-22:** Cấu hình phí ship: bảng giá theo khu vực (tỉnh/huyện), theo khoảng trọng lượng, ngưỡng miễn phí ship<br>**BR-11:** Coupon có thể stack hoặc không stack (cấu hình)<br>**BR-12:** Ngưỡng miễn phí 500k & bảng phí ship cần chủ shop duyệt trước khi deploy | UC-16: Quản lý khuyến mãi & phí ship | Cao |
| **CS-03** | Chủ cửa hàng: báo cáo doanh thu, số lượng đơn, top sản phẩm, chuyển đổi | Phỏng vấn chủ shop (câu 4) | **FR-23:** Dashboard: Doanh thu theo ngày/tuần/tháng/năm, số đơn, AOV, tỷ lệ chuyển đổi (khách vào web → đặt hàng)<br>**FR-24:** Báo cáo Top sản phẩm bán chạy, tồn kho thấp, đơn hàng theo trạng thái<br>**FR-25:** Xuất báo cáo Excel/PDF (tuần/tháng)<br>**NFR-07:** Dữ liệu báo cáo cập nhật near real-time (cron 5p hoặc trigger khi đơn đổi trạng thái) | UC-17: Báo cáo & Dashboard | Trung bình |
| **CS-04** | Chủ cửa hàng: phân quyền tài khoản nhân viên (nhân viên bán hàng, kho, kế toán) | Phạm vi (quản lý tài khoản và phân quyền) | **FR-26:** Quản lý user nội bộ: CRUD, gán role (Admin / Nhân viên bán / Nhân viên kho / Kế toán)<br>**FR-27:** Phân quyền theo màn hình/chức năng (RBAC)<br>**BR-13:** Admin không thể tự gán role Admin cho mình (cần super-admin hoặc config file) | UC-18: Quản trị người dùng & phân quyền | Cao |
| **PG-01** | Cổng thanh toán: nhận request thanh toán, trả về kết quả (success/fail/pending), callback/IPN | Phạm vi (cổng thanh toán mô phỏng) | **FR-28:** API endpoint tạo giao dịch: amount, orderId, returnUrl, callbackUrl<br>**FR-29:** Trang thanh toán mô phỏng: hiển thị số tiền, nút "Thành công" / "Thất bại" / "Pending" để tester chọn<br>**FR-30:** Callback/IPN giả lập: POST về `/api/payment/callback` với signature verification<br>**NFR-08:** Tuân thủ chuẩn VNPAY/MoMo/API ngân hàng (chỉ mô phỏng interface) | UC-19: Tích hợp cổng thanh toán (mô phỏng) | Cao |
| **DV-01** | Đơn vị vận chuyển: cập nhật trạng thái giao hàng (đã lấy hàng, đang giao, giao thành công, giao thất bại, trả hàng) | Quy trình To-Be | **FR-31:** API endpoint cập nhật trạng thái vận đơn: mã vận đơn, trạng thái mới, timestamp, ghi chú, hình ảnh chứng từ (tùy chọn)<br>**FR-32:** Webhook/UI cho nhân viên hãng ship cập nhật trạng thái thủ công (mô phỏng)<br>**BR-14:** Trạng thái hợp lệ: `PICKED_UP`, `IN_TRANSIT`, `DELIVERED`, `FAILED`, `RETURNED`<br>**FR-33:** Tự động cập nhật đơn hàng bên hệ thống khi nhận webhook | UC-20: Cập nhật trạng thái giao hàng (hãng ship) | Cao |

---

## 2. Phân loại yêu cầu theo nhóm chức năng

| Nhóm | Mã yêu cầu | Số lượng | Ghi chú |
|------|------------|----------|---------|
| **Khách hàng (Customer-facing)** | FR-01 ~ FR-11, NFR-01~04, BR-01~05 | 11 FR + 4 NFR + 5 BR | Core business flow |
| **Nhân viên vận hành (Staff-facing)** | FR-12 ~ FR-18, NFR-05, BR-06~09 | 7 FR + 1 NFR + 4 BR | Back-office operations |
| **Quản trị/Chủ shop (Admin-facing)** | FR-19 ~ FR-27, NFR-06~07, BR-10~13 | 9 FR + 2 NFR + 4 BR | Quản trị hệ thống |
| **Tích hợp bên thứ 3 (Integration)** | FR-28 ~ FR-33, NFR-08, BR-14 | 6 FR + 1 NFR + 1 BR | Payment & Shipping |
| **Tổng cộng** | | **33 FR + 8 NFR + 14 BR** | |

---

## 3. Danh sách Use Case tổng hợp (gán mã UC thống nhất)

| Mã UC | Tên Use Case | Actor chính | Mô tả tóm tắt | Ưu tiên |
|-------|--------------|-------------|---------------|---------|
| UC-01 | Xem danh sách sản phẩm | Khách | Duyệt, phân trang, xem gird/list | Cao |
| UC-02 | Tìm kiếm & lọc sản phẩm | Khách | Tìm kiếm từ khóa, lọc đa tiêu chí | Cao |
| UC-03 | Xem chi tiết sản phẩm | Khách | Hình ảnh, mô tả, thuộc tính, tồn kho, review (nếu có) | Cao |
| UC-04 | Quản lý giỏ hàng | Khách | Thêm/sửa/xóa, xem tóm tắt, lưu localStorage | Cao |
| UC-05 | Đăng ký tài khoản | Khách | Email/SĐT + OTP, profile cơ bản | Cao |
| UC-06 | Đăng nhập/Đăng xuất | Khách | JWT, remember me, quên mật khẩu | Cao |
| UC-07 | Quản lý hồ sơ cá nhân | Khách | Địa chỉ lưu, đổi mật khẩu, lịch sử đơn | Cao |
| UC-08 | Thanh toán & đặt hàng | Khách | Checkout 1 trang, validate toàn diện | Cao |
| UC-09 | Tra cứu & theo dõi đơn hàng | Khách | Timeline, mã vận đơn, thông báo | Cao |
| UC-10 | Thanh toán đơn hàng | Khách | COD / Chuyển khoản / Cổng mô phỏng | Cao |
| UC-11 | Xác nhận đơn hàng | Nhân viên | Kiểm tra tồn kho, địa chỉ, coupon, phí ship | Cao |
| UC-12 | Đóng gói & tạo vận đơn | Nhân viên | Check sản phẩm, in phiếu, sinh mã vận đơn | Cao |
| UC-13 | Xử lý ngoại lệ đơn hàng | Nhân viên | Thanh toán lỗi, giao thất bại, hủy đơn | Cao |
| UC-14 | Quản lý danh mục & sản phẩm | Quản trị | CRUD đầy đủ, upload ảnh, SKU, thuộc tính | Cao |
| UC-15 | Quản lý tồn kho | Quản trị | Nhập/xuất, điều chỉnh, cảnh báo, lịch sử | Cao |
| UC-16 | Quản lý khuyến mãi & phí ship | Quản trị | Coupon, bảng phí ship, ngưỡng miễn phí | Cao |
| UC-17 | Báo cáo & Dashboard | Quản trị | Doanh thu, đơn, top SP, xuất Excel/PDF | Trung bình |
| UC-18 | Quản trị người dùng & phân quyền | Quản trị | CRUD user, RBAC, gán role | Cao |
| UC-19 | Tích hợp cổng thanh toán (mô phỏng) | Cổng TT | Tạo giao dịch, trang thanh toán, callback | Cao |
| UC-20 | Cập nhật trạng thái giao hàng | Hãng ship | API/Webhook cập nhật trạng thái vận đơn | Cao |

---

## 4. Ghi chú truy vết (Traceability Notes)

1. **Mã truy vết**: Mỗi yêu cầu có mã duy nhất (FR/NFR/BR-xx) liên kết ngược về STT trong bảng ma trận và tiến tới UC-xx.
2. **Nguồn chứng thực**: Mọi yêu cầu bắt buộc ghi rõ nguồn (Phỏng vấn buổi X / Bảng hỏi câu Y / Benchmark website Z / Phạm vi đề tài / Giả định 1.5).
3. **Người xác nhận**: Chủ shop xác nhận BR-04, BR-05, BR-11, BR-12; Nhân viên xác nhận BR-06, BR-07, BR-08, BR-09; Kỹ thuật xác nhận NFR, BR kỹ thuật.
4. **Không tự suy diễn**: Số lượng người phỏng vấn, tỷ lệ phản hồi bảng hỏi, điểm benchmark đều phải có bằng chứng thực tế (biên bản, export Google Form, chụp màn hình competitor).
5. **Cập nhật**: Bảng này là *living document* — mỗi khi có yêu cầu mới/sửa đổi, cập nhật STT, version, ngày, người cập nhật.

---

## 5. Template ghi nhận yêu cầu mới (dùng khi mở rộng)

| Trường | Mô tả |
|--------|-------|
| **STT** | Tự động tăng |
| **Nhu cầu** | Câu chuyện người dùng (As a... I want... So that...) |
| **Nguồn** | Phỏng vấn/Bảng hỏi/Benchmark/Phạm vi/Giả định/Stakeholder đề xuất |
| **Ngày thu thập** | DD/MM/YYYY |
| **Người thu thập** | Tên thành viên nhóm |
| **Người xác nhận** | Tên stakeholder ký tên |
| **Yêu cầu hệ thống** | FR-xx / NFR-xx / BR-xx (mô tả chi tiết, tiêu chí chấp nhận) |
| **UC liên quan** | UC-xx, UC-yy |
| **Mức ưu tiên** | Cao / Trung bình / Thấp (MoSCoW: Must/Should/Could/Won't) |
| **Trạng thái** | Đã xác nhận / Đang phân tích / Đã triển khai / Đã test / Đã nghiệm thu |
| **Ghi chú** | Ràng buộc, phụ thuộc, rủi ro, giả định cần verify |

---

> **Lưu ý:** File này là *đầu ra* của giai đoạn khảo sát (Chương 2 báo cáo). Nội dung sẽ được tham chiếu trực tiếp khi viết **Chương 3 – Yêu cầu hệ thống** và xây dựng **Ma trận truy vết (RTM)** ở các giai đoạn sau.
