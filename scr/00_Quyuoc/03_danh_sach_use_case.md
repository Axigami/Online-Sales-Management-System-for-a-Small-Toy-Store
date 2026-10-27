# Danh sách Use Case (25 UC)

★ = ưu tiên làm trước.

| Mã | Use Case | Actor | Ghi chú / quan hệ |
| --- | --- | --- | --- |
| **A. Tài khoản** | | | |
| UC01 ★ | Đăng ký tài khoản | Guest | |
| UC02 ★ | Đăng nhập / Đăng xuất | Customer, Staff, Admin | UC13 «include» UC02 |
| UC03 | Quản lý hồ sơ & địa chỉ giao hàng | Customer | |
| UC04 | Quên / đổi mật khẩu | Customer | |
| **B. Sản phẩm & kho** | | | |
| UC05 ★ | Xem, tìm kiếm, lọc sản phẩm (danh mục, độ tuổi, giá) | Guest | |
| UC06 | Xem chi tiết sản phẩm | Guest | |
| UC07 ★ | Quản lý sản phẩm (thêm / sửa / ẩn / xóa) | Admin | |
| UC08 | Quản lý danh mục | Admin | |
| UC09 | Quản lý tồn kho (nhập kho, điều chỉnh) | Staff, Admin | Ghi INVENTORY_LOG |
| **C. Giỏ hàng** | | | |
| UC10 ★ | Thêm sản phẩm vào giỏ | Guest, Customer | Có bước kiểm tra tồn kho |
| UC11 ★ | Xem / sửa số lượng / xóa sản phẩm trong giỏ | Guest, Customer | |
| UC12 | Áp dụng mã giảm giá | Customer | «extend» UC13 |
| **D. Đặt hàng & thanh toán** | | | |
| UC13 ★ | Đặt hàng (checkout) | Customer | «include» UC02, UC20; «extend» UC12, UC14 |
| UC14 ★ | Thanh toán online | Customer, Cổng thanh toán | «extend» UC13 (khi chọn thanh toán online) |
| UC15 ★ | Xem lịch sử & theo dõi đơn hàng | Customer | |
| UC16 | Hủy đơn hàng | Customer | Chỉ khi chưa đóng gói |
| **E. Xử lý đơn (back-office)** | | | |
| UC17 ★ | Xem & xác nhận đơn hàng | Staff, Admin | |
| UC18 ★ | Cập nhật trạng thái đơn (đóng gói, hoàn tất) | Staff | |
| UC19 | Xử lý đổi trả / hoàn tiền (mở rộng) | Admin | Làm nếu còn thời gian |
| **F. Vận chuyển** | | | |
| UC20 | Tính phí vận chuyển | Đơn vị vận chuyển | «include» trong UC13 |
| UC21 ★ | Tạo vận đơn, bàn giao cho đơn vị vận chuyển | Staff, Đơn vị vận chuyển | |
| UC22 ★ | Theo dõi & cập nhật trạng thái giao hàng | Customer, Staff, Đơn vị vận chuyển | |
| **G. Quản trị khác** | | | |
| UC23 | Quản lý mã giảm giá / khuyến mãi | Admin | |
| UC24 | Thống kê doanh thu & đơn hàng | Admin | |
| UC25 | Quản lý tài khoản & phân quyền | Admin | |

## Quan hệ đã chốt (phải thể hiện đúng trên Use Case Diagram)

- Generalization: **Customer → Guest**, **Admin → Staff**.
- UC13 «include» UC02, UC20.
- UC13 «extend» UC12, UC14.
