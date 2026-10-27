# Phạm vi và actor

## 1. Bối cảnh và phạm vi

- **Bối cảnh:** cửa hàng đồ chơi nhỏ (1 chủ + 1–2 nhân viên), hiện nhận đơn qua Facebook/Zalo/tại quầy, ghi sổ hoặc Excel, tự gọi shipper.
- **Vấn đề:** sót/nhầm đơn; tồn kho lệch; khó theo dõi giao hàng; khách không tự xem được đơn → cần website bán hàng + trang quản trị.
- **Trong phạm vi:** sản phẩm & danh mục, giỏ hàng, đặt hàng, thanh toán (COD, chuyển khoản, cổng thanh toán – mô phỏng), vận chuyển (tính phí, tạo vận đơn, theo dõi), tồn kho cơ bản, mã giảm giá, thống kê cơ bản.
- **Ngoài phạm vi:** app mobile, chat/CSKH, đa chi nhánh, đánh giá sản phẩm, kết nối thật với cổng thanh toán/API vận chuyển.

## 2. Actor (6 actor: 4 người + 2 hệ thống ngoài)

| Actor | Loại | Làm gì |
| --- | --- | --- |
| Khách vãng lai (Guest) | Người | Xem/tìm sản phẩm, thêm vào giỏ, đăng ký tài khoản |
| Khách hàng (Customer) | Người | Kế thừa Guest + đặt hàng, thanh toán, theo dõi/hủy đơn, quản lý hồ sơ & địa chỉ |
| Nhân viên (Staff) | Người | Xác nhận đơn, đóng gói, tạo vận đơn, cập nhật trạng thái, nhập kho |
| Chủ cửa hàng / Quản trị (Admin) | Người | Kế thừa Staff + quản lý sản phẩm, danh mục, mã giảm giá, tài khoản, xem báo cáo |
| Cổng thanh toán (Payment Gateway) | Hệ thống ngoài | Xử lý thanh toán online, trả kết quả thành công/thất bại |
| Đơn vị vận chuyển (Carrier, vd GHN/GHTK) | Hệ thống ngoài | Nhận vận đơn, trả phí ship, cập nhật trạng thái giao hàng |

Quan hệ kế thừa (generalization): **Customer → Guest**, **Admin → Staff**.
