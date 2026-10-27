# SD4 – Cập nhật trạng thái giao hàng (UC22)

> **Phụ trách:** Huỳnh Nguyễn Quốc Việt  
> **Trạng thái:** 🔴 Chưa làm  
> **Hạn:** —

<!-- huong-dan:start -->
> 💡 **Lifeline:** Carrier, ShipmentController (webhook), ShipmentService, OrderService, NotificationService, Khách
> **Thông điệp chính:** Carrier gửi trạng thái → lưu Shipment → cập nhật Order → gửi thông báo → khách xem đơn (SD4 minh họa Carrier gọi ngược về hệ thống – callback)
> Dùng combined fragment `alt/opt/loop` cho các nhánh. **Tên thông điệp phải khớp tên phương thức trong Class Diagram của Quang.**
> Lưu `hinh/sd4_ten.drawio` + `.png`.
<!-- huong-dan:end -->

## Sơ đồ

TODO (chèn hình)

## Giải thích (3–5 dòng)

TODO

## Đối chiếu với Class Diagram

| Thông điệp trong sơ đồ | Lớp | Phương thức trong Class Diagram |
| --- | --- | --- |
| TODO | TODO | TODO |
