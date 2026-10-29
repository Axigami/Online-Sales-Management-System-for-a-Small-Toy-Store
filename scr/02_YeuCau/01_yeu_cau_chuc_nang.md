# Yêu cầu chức năng (FR)

> **Phụ trách:** Tô Đoàn Khánh Nguyên  
> **Trạng thái:** 🟠 Chờ duyệt  
> **Hạn:** 2026-11-05  

Tài liệu đặc tả các yêu cầu chức năng (Functional Requirements - FR) cho **Hệ thống quản lý bán hàng trực tuyến cho cửa hàng đồ chơi nhỏ**. Toàn bộ yêu cầu được chuẩn hóa theo cú pháp `Hệ thống cho phép [Actor] [hành động]`, xếp mức ưu tiên theo phương pháp **MoSCoW** (Must have, Should have, Could have) và ánh xạ trực tiếp đến [Danh sách 25 Use Case](../00_Quyuoc/03_danh_sach_use_case.md) của dự án.

---

## 1. Phân hệ Tài khoản & Phân quyền

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR01** | Hệ thống cho phép Khách vãng lai (Guest) đăng ký tài khoản mới bằng thông tin email/số điện thoại, họ tên và mật khẩu. | Must | UC01 |
| **FR02** | Hệ thống cho phép Người dùng (Customer, Staff, Admin) đăng nhập vào hệ thống bằng tài khoản đã cấp và đăng xuất an toàn khỏi phiên làm việc. | Must | UC02 |
| **FR03** | Hệ thống cho phép Khách hàng (Customer) xem, chỉnh sửa hồ sơ cá nhân và quản lý danh sách nhiều địa chỉ nhận hàng (thêm, sửa, xóa, chọn địa chỉ mặc định). | Must | UC03 |
| **FR04** | Hệ thống cho phép Khách hàng (Customer) yêu cầu cấp lại mật khẩu qua email khi bị quên và tự thực hiện đổi mật khẩu sau khi đã đăng nhập. | Should | UC04 |
| **FR05** | Hệ thống cho phép Quản trị viên (Admin) quản lý danh sách tài khoản nội bộ và phân quyền truy cập theo vai trò (Admin, Staff) theo mô hình RBAC. | Must | UC25 |

---

## 2. Phân hệ Quản lý Sản phẩm & Danh mục

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR06** | Hệ thống cho phép Khách vãng lai xem danh sách sản phẩm đồ chơi, tìm kiếm theo từ khóa tên sản phẩm và lọc theo danh mục, độ tuổi, khoảng giá, thương hiệu có hỗ trợ phân trang. | Must | UC05 |
| **FR07** | Hệ thống cho phép Khách vãng lai xem thông tin chi tiết một sản phẩm đồ chơi (hình ảnh đại diện & chi tiết, tên, mã SKU, thương hiệu, độ tuổi khuyến nghị, giá bán, mô tả chất liệu và số lượng tồn kho). | Must | UC06 |
| **FR08** | Hệ thống cho phép Quản trị viên (Admin) quản lý cây danh mục sản phẩm (thêm mới, chỉnh sửa thông tin, ẩn hoặc xóa danh mục đồ chơi). | Must | UC08 |
| **FR09** | Hệ thống cho phép Quản trị viên (Admin) quản lý thông tin sản phẩm đồ chơi (thêm sản phẩm mới, cập nhật giá/hình ảnh/thuộc tính độ tuổi, ẩn hoặc xóa sản phẩm khỏi danh mục hiển thị). | Must | UC07 |
| **FR10** | Hệ thống cho phép Nhân viên (Staff) và Admin quản lý việc nhập kho, điều chỉnh số lượng tồn kho thực tế của từng sản phẩm và tự động lưu nhật ký biến động kho (`INVENTORY_LOG`). | Must | UC09 |
| **FR11** | Hệ thống tự động phát tín hiệu cảnh báo trên giao diện quản trị khi số lượng tồn kho của một mã sản phẩm giảm xuống dưới ngưỡng an toàn cấu hình (ví dụ: tồn kho ≤ 5). | Should | UC09 |

---

## 3. Phân hệ Quản lý Giỏ hàng

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR12** | Hệ thống cho phép Khách vãng lai và Khách hàng thêm sản phẩm vào giỏ hàng với số lượng tùy chọn sau khi hệ thống kiểm tra và xác nhận tồn kho thực tế đáp ứng đủ. | Must | UC10 |
| **FR13** | Hệ thống cho phép Người dùng xem danh sách sản phẩm trong giỏ, tăng/giảm số lượng từng món hoặc xóa món đồ chơi khỏi giỏ; hệ thống tự động cập nhật lại tổng tạm tính. | Must | UC11 |
| **FR14** | Hệ thống cho phép lưu giữ thông tin giỏ hàng của Khách vãng lai trong phiên duyệt web (LocalStorage/Session) và tự động gộp vào giỏ hàng tài khoản ngay sau khi người dùng đăng nhập. | Must | UC11 |

---

## 4. Phân hệ Đặt hàng & Thanh toán

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR15** | Hệ thống cho phép Khách hàng (Customer) áp dụng mã giảm giá (coupon) hợp lệ vào giỏ hàng; hệ thống tự động kiểm tra điều kiện áp dụng (giá trị đơn tối thiểu, thời hạn, số lượt) và tính chiết khấu. | Should | UC12 |
| **FR16** | Hệ thống cho phép Khách hàng tiến hành đặt hàng (checkout): chọn hoặc nhập địa chỉ giao hàng, lựa chọn phương thức vận chuyển và lựa chọn phương thức thanh toán (COD, chuyển khoản, cổng trực tuyến). | Must | UC13 |
| **FR17** | Hệ thống tự động tính toán tổng tiền thanh toán = (Tổng tiền hàng) + (Phí ship) – (Giảm giá coupon), đồng thời tự động kiểm tra lại tồn kho tức thời trước khi chốt tạo đơn hàng. | Must | UC13, UC20 |
| **FR18** | Hệ thống cho phép Khách hàng thanh toán trực tuyến qua Cổng thanh toán mô phỏng (hỗ trợ chuyển hướng sang trang thanh toán mô phỏng và tiếp nhận kết quả xử lý thành công hoặc thất bại). | Must | UC14 |
| **FR19** | Hệ thống cho phép Khách hàng xem danh sách lịch sử đơn hàng, xem chi tiết tiến trình vận đơn, thông tin người nhận và danh mục sản phẩm của từng đơn hàng đã đặt. | Must | UC15 |
| **FR20** | Hệ thống cho phép Khách hàng chủ động gửi yêu cầu hủy đơn hàng trực tiếp trên hệ thống nếu đơn hàng vẫn đang ở trạng thái "Chờ xác nhận" (chưa chuyển sang đóng gói). | Should | UC16 |

---

## 5. Phân hệ Xử lý đơn hàng & Quản lý Kho

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR21** | Hệ thống cho phép Nhân viên (Staff) và Admin xem danh sách đơn hàng theo từng trạng thái xử lý, xem chi tiết từng đơn và thực hiện xác nhận hoặc từ chối đơn hàng. | Must | UC17 |
| **FR22** | Hệ thống tự động trừ số lượng tồn kho khả dụng ngay khi đơn hàng được tạo thành công, và tự động hoàn trả lại số lượng tồn kho khi đơn hàng bị hủy hoặc giao thất bại chuyển hoàn. | Must | UC13, UC17 |
| **FR23** | Hệ thống cho phép Nhân viên cập nhật trạng thái đơn hàng (Chờ xác nhận → Đã xác nhận → Đang đóng gói → Đã giao) và ghi vết lịch sử thay đổi trạng thái kèm định danh nhân viên và thời điểm thực hiện. | Must | UC18 |
| **FR24** | Hệ thống cho phép Quản trị viên (Admin) tiếp nhận và xử lý yêu cầu đổi trả hàng hoặc hoàn tiền cho khách hàng đối với các đơn hàng gặp sự cố sản phẩm hoặc giao hàng. | Could | UC19 |

---

## 6. Phân hệ Vận chuyển & Giao nhận

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR25** | Hệ thống tự động tính cước phí vận chuyển dựa trên địa chỉ giao hàng (khu vực tỉnh/huyện) và tổng khối lượng quy cách đơn hàng theo biểu cước cấu hình (hoặc miễn phí ship với đơn từ 500.000đ). | Must | UC20 |
| **FR26** | Hệ thống cho phép Nhân viên tạo vận đơn giao hàng mô phỏng (chọn đơn vị vận chuyển GHN/GHTK, hệ thống tự động sinh mã vận đơn theo định dạng `WH-YYYYMMDD-XXXX`) và chuyển trạng thái đơn sang "Đang giao". | Must | UC21 |
| **FR27** | Hệ thống cho phép tiếp nhận thông tin cập nhật trạng thái giao hàng từ Đơn vị vận chuyển (thông qua API/Webhook mô phỏng) để tự động đồng bộ trạng thái đơn hàng (Đang giao → Đã giao / Giao thất bại / Hoàn hàng). | Must | UC22 |

---

## 7. Phân hệ Báo cáo & Quản trị Khuyến mãi

| Mã FR | Mô tả (Hệ thống cho phép [actor] [làm gì]) | MoSCoW | UC |
| :--- | :--- | :---: | :---: |
| **FR28** | Hệ thống cho phép Quản trị viên (Admin) quản lý các mã giảm giá và chương trình khuyến mãi (tạo mã mới, quy định mức giảm % hoặc số tiền cố định, đơn tối thiểu, thời gian hiệu lực và giới hạn lượt dùng). | Should | UC23 |
| **FR29** | Hệ thống cho phép Quản trị viên (Admin) xem dashboard biểu đồ thống kê doanh thu theo ngày/tuần/tháng, tổng số đơn hàng theo từng trạng thái và danh sách top sản phẩm đồ chơi bán chạy nhất. | Must | UC24 |
| **FR30** | Hệ thống cho phép Quản trị viên xuất dữ liệu báo cáo thống kê doanh thu và báo cáo danh sách đơn hàng ra định dạng file Excel/PDF để lưu trữ nội bộ. | Could | UC24 |
