# Từ điển dữ liệu (6 bảng chính) và ràng buộc

> **Phụ trách:** Trần Vinh Quang  
> **Trạng thái:** 🟠 Chờ duyệt
> **Hạn:** —

<!-- huong-dan:start -->
> 💡 Tên cột lấy từ ERD (đã bỏ dấu). Điền kiểu dữ liệu, ràng buộc (NOT NULL, UNIQUE, CHECK, DEFAULT…) và mô tả cho từng cột. Trạng thái dùng danh sách giá trị đã chốt ở [Trạng thái và quy tắc nghiệp vụ](../00_QuyUoc/02_trang_thai_va_quy_tac_nghiep_vu.md).
<!-- huong-dan:end -->

## Bảng PRODUCT

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | INT | PK, AUTO_INCREMENT | Mã sản phẩm |
| category_id | INT | FK → CATEGORY.id, NOT NULL | Danh mục chứa sản phẩm |
| ten | VARCHAR(200) | NOT NULL | Tên sản phẩm |
| mo_ta | TEXT | NULL được | Mô tả chi tiết sản phẩm |
| gia | DECIMAL(12,2) | NOT NULL, CHECK (gia >= 0) | Giá bán hiện tại (VNĐ) |
| ton_kho | INT | NOT NULL, DEFAULT 0, CHECK (ton_kho >= 0) | Số lượng còn trong kho |
| do_tuoi | VARCHAR(20) | NULL được | Độ tuổi khuyến nghị, ví dụ 3+, 6+ |
| khoi_luong | DECIMAL(8,2) | NOT NULL, CHECK (khoi_luong > 0) | Khối lượng (kg), dùng tính phí vận chuyển |
| trang_thai | VARCHAR(20) | NOT NULL, DEFAULT 'ACTIVE', CHECK IN ('ACTIVE','HIDDEN','OUT_OF_STOCK') | Trạng thái hiển thị và kinh doanh của sản phẩm |

## Bảng CART_ITEM

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | INT | PK, AUTO_INCREMENT | Mã dòng giỏ hàng |
| cart_id | INT | FK → CART.id, NOT NULL, ON DELETE CASCADE | Giỏ hàng chứa dòng này |
| product_id | INT | FK → PRODUCT.id, NOT NULL, UNIQUE (cart_id, product_id) | Sản phẩm trong giỏ, không trùng trong cùng một giỏ |
| so_luong | INT | NOT NULL, DEFAULT 1, CHECK (so_luong > 0) | Số lượng khách muốn mua |

## Bảng ORDERS

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | INT | PK, AUTO_INCREMENT | Mã đơn hàng |
| user_id | INT | FK → USER.id, NOT NULL | Khách đặt hàng (bắt buộc đăng nhập, BR1) |
| address_id | INT | FK → ADDRESS.id, NOT NULL | Địa chỉ giao hàng |
| coupon_id | INT | FK → COUPON.id, NULL được | Mã giảm giá áp dụng, tối đa 1 mã mỗi đơn (BR5) |
| tong_tien | DECIMAL(12,2) | NOT NULL, CHECK (tong_tien >= 0) | Tổng tiền khách phải trả = tiền hàng + phi_ship − giam_gia |
| phi_ship | DECIMAL(12,2) | NOT NULL, DEFAULT 0, CHECK (phi_ship >= 0) | Phí vận chuyển tính cho khách, bằng 0 khi đạt ngưỡng miễn phí (BR4) |
| giam_gia | DECIMAL(12,2) | NOT NULL, DEFAULT 0, CHECK (giam_gia >= 0) | Số tiền được giảm nhờ mã giảm giá |
| trang_thai | VARCHAR(20) | NOT NULL, DEFAULT 'PENDING', CHECK IN ('PENDING','CONFIRMED','PACKING','SHIPPING','DELIVERED','CANCELLED','FAILED') (xem quy ước) | Trạng thái xử lý đơn hàng |
| ngay_tao | DATETIME | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Thời điểm đặt đơn |

## Bảng ORDER_ITEM

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | INT | PK, AUTO_INCREMENT | Mã dòng đơn hàng |
| order_id | INT | FK → ORDERS.id, NOT NULL, ON DELETE CASCADE | Đơn hàng chứa dòng này |
| product_id | INT | FK → PRODUCT.id, NOT NULL | Sản phẩm được mua |
| so_luong | INT | NOT NULL, CHECK (so_luong > 0) | Số lượng mua |
| don_gia | DECIMAL(12,2) | NOT NULL, CHECK (don_gia >= 0), giá tại thời điểm mua (BR3) | Đơn giá lưu cố định lúc đặt, không đổi khi PRODUCT.gia thay đổi |

## Bảng PAYMENT

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | INT | PK, AUTO_INCREMENT | Mã thanh toán |
| order_id | INT | FK → ORDERS.id, NOT NULL, UNIQUE | Đơn hàng được thanh toán, mỗi đơn có đúng 1 thanh toán |
| phuong_thuc | VARCHAR(20) | NOT NULL, CHECK IN ('COD','BANK_TRANSFER','GATEWAY') (COD / chuyển khoản / cổng thanh toán, BR6) | Phương thức thanh toán |
| so_tien | DECIMAL(12,2) | NOT NULL, CHECK (so_tien >= 0) | Số tiền cần thanh toán |
| trang_thai | VARCHAR(20) | NOT NULL, DEFAULT 'UNPAID', CHECK IN ('UNPAID','PAID','REFUNDED','FAILED') (xem quy ước) | Trạng thái thanh toán |
| ma_giao_dich | VARCHAR(100) | NULL được | Mã giao dịch ngân hàng hoặc cổng thanh toán, để trống với COD |
| thoi_gian | DATETIME | NULL được | Thời điểm thanh toán thành công, để trống khi chưa thanh toán |

## Bảng SHIPMENT

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | INT | PK, AUTO_INCREMENT | Mã vận đơn nội bộ |
| order_id | INT | FK → ORDERS.id, NOT NULL, UNIQUE | Đơn hàng được giao, mỗi đơn có tối đa 1 vận đơn (chỉ tạo sau khi xác nhận) |
| carrier_id | INT | FK → CARRIER.id, NOT NULL | Đơn vị vận chuyển phụ trách |
| ma_van_don | VARCHAR(50) | NOT NULL, UNIQUE | Mã vận đơn dùng để tra cứu hành trình |
| phi | DECIMAL(12,2) | NOT NULL, DEFAULT 0, CHECK (phi >= 0) | Phí vận chuyển tính từ bảng CARRIER |
| trang_thai | VARCHAR(20) | NOT NULL, DEFAULT 'CREATED', CHECK IN ('CREATED','PICKED_UP','IN_TRANSIT','DELIVERED','FAILED','RETURNED') (xem quy ước) | Trạng thái giao hàng |
| ngay_giao | DATE | NULL được | Ngày giao thành công, để trống khi chưa giao xong |

## Danh sách ràng buộc toàn cục

- Tồn kho ≥ 0 (PRODUCT.ton_kho).
- Đơn giá lưu ở ORDER_ITEM (giá tại thời điểm mua, BR3).
- Các cột trạng thái dùng danh sách giá trị cố định (trạng thái đơn, thanh toán, vận đơn).
- Email người dùng là duy nhất (USER.email).
- Quan hệ 1–1: mỗi USER có đúng 1 CART (CART.user_id UNIQUE); mỗi ORDERS có đúng 1 PAYMENT (PAYMENT.order_id UNIQUE); mỗi ORDERS có tối đa 1 SHIPMENT (SHIPMENT.order_id UNIQUE, vận đơn chỉ tạo sau khi xác nhận đơn).
- Một sản phẩm không xuất hiện hai lần trong cùng một giỏ: UNIQUE (CART_ITEM.cart_id, CART_ITEM.product_id).
- Số lượng phải dương: CART_ITEM.so_luong > 0, ORDER_ITEM.so_luong > 0; khối lượng sản phẩm > 0.
- Các cột tiền không âm: PRODUCT.gia, ORDERS.tong_tien, ORDERS.phi_ship, ORDERS.giam_gia, ORDER_ITEM.don_gia, PAYMENT.so_tien, SHIPMENT.phi (≥ 0).
- Mã giảm giá: COUPON.ma là duy nhất; chỉ áp dụng khi còn hạn (han_dung ≥ ngày đặt), còn lượt (so_luot > 0) và đơn đạt don_toi_thieu; mỗi lần dùng giảm so_luot đi 1; mỗi đơn dùng tối đa 1 mã (ORDERS.coupon_id, BR5).
- Mã vận đơn là duy nhất (SHIPMENT.ma_van_don); tên danh mục và tên đơn vị vận chuyển không trùng (CATEGORY.ten, CARRIER.ten).
- Mỗi người dùng có tối đa 1 địa chỉ mặc định (ADDRESS.mac_dinh).
- Đặt hàng bắt buộc đăng nhập: ORDERS.user_id NOT NULL (BR1).
- Chỉ được hủy đơn khi chưa đóng gói; hủy đơn hoặc giao thất bại thì hoàn lại tồn kho và ghi INVENTORY_LOG.
- Toàn vẹn tham chiếu: mọi khóa ngoại phải trỏ tới bản ghi tồn tại; xóa bản ghi cha thì xóa luôn chi tiết (ON DELETE CASCADE cho ADDRESS, PRODUCT_IMAGE, CART_ITEM, ORDER_ITEM).
- Miễn phí vận chuyển khi tiền hàng từ 500.000đ trở lên (BR4, số giả định).
