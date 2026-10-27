# Trạng thái và quy tắc nghiệp vụ

## 1. Trạng thái

- **Trạng thái đơn:** Chờ xác nhận → Đã xác nhận → Đang đóng gói → Đang giao → Đã giao. Nhánh: **Đã hủy** (chỉ khi chưa đóng gói); **Giao thất bại → Hoàn về kho**.
- **Trạng thái thanh toán:** Chưa thanh toán / Đã thanh toán / Hoàn tiền.
- **Trạng thái vận đơn:** Chờ lấy hàng / Đang vận chuyển / Đã giao / Giao thất bại / Hoàn hàng.

Các cột `trang_thai` trong CSDL phải dùng **danh sách giá trị cố định** đúng như trên.

## 2. Quy tắc nghiệp vụ

| Mã | Nội dung |
| --- | --- |
| BR1 | Đặt hàng bắt buộc đăng nhập; giỏ của khách vãng lai được gộp vào giỏ sau khi đăng nhập. |
| BR2 | Kiểm tra tồn kho khi thêm giỏ và khi đặt hàng; trừ tồn kho khi tạo đơn thành công; hoàn tồn kho khi hủy/hoàn hàng. |
| BR3 | Giá trong đơn là giá tại thời điểm mua (lưu ở ORDER_ITEM). |
| BR4 | Phí ship = phí cơ bản theo khu vực + theo khối lượng; miễn phí ship cho đơn từ 500.000đ (số giả định, nhóm có thể chỉnh nhưng phải thống nhất). |
| BR5 | Mã giảm giá có hạn dùng, số lượt, đơn tối thiểu; mỗi đơn dùng tối đa 1 mã. |
| BR6 | Phương thức thanh toán: COD, chuyển khoản, cổng thanh toán (mô phỏng). |
