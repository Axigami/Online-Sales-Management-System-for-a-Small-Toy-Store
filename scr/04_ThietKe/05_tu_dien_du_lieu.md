# Từ điển dữ liệu (6 bảng chính) và ràng buộc

> **Phụ trách:** Trần Vinh Quang  
> **Trạng thái:** 🔴 Chưa làm  
> **Hạn:** —

<!-- huong-dan:start -->
> 💡 Tên cột lấy từ ERD (đã bỏ dấu). Điền kiểu dữ liệu, ràng buộc (NOT NULL, UNIQUE, CHECK, DEFAULT…) và mô tả cho từng cột. Trạng thái dùng danh sách giá trị đã chốt ở [Trạng thái và quy tắc nghiệp vụ](../00_QuyUoc/02_trang_thai_va_quy_tac_nghiep_vu.md).
<!-- huong-dan:end -->

## Bảng PRODUCT

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | TODO | PK | TODO |
| category_id | TODO | FK → CATEGORY.id | TODO |
| ten | TODO | TODO | TODO |
| mo_ta | TODO | TODO | TODO |
| gia | TODO | TODO | TODO |
| ton_kho | TODO | CHECK (ton_kho >= 0) | TODO |
| do_tuoi | TODO | TODO | TODO |
| khoi_luong | TODO | TODO | TODO |
| trang_thai | TODO | TODO | TODO |

## Bảng CART_ITEM

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | TODO | PK | TODO |
| cart_id | TODO | FK → CART.id | TODO |
| product_id | TODO | FK → PRODUCT.id | TODO |
| so_luong | TODO | TODO | TODO |

## Bảng ORDERS

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | TODO | PK | TODO |
| user_id | TODO | FK → USER.id | TODO |
| address_id | TODO | FK → ADDRESS.id | TODO |
| coupon_id | TODO | FK → COUPON.id | TODO |
| tong_tien | TODO | TODO | TODO |
| phi_ship | TODO | TODO | TODO |
| giam_gia | TODO | TODO | TODO |
| trang_thai | TODO | Giá trị cố định (xem quy ước) | TODO |
| ngay_tao | TODO | TODO | TODO |

## Bảng ORDER_ITEM

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | TODO | PK | TODO |
| order_id | TODO | FK → ORDERS.id | TODO |
| product_id | TODO | FK → PRODUCT.id | TODO |
| so_luong | TODO | TODO | TODO |
| don_gia | TODO | Giá tại thời điểm mua (BR3) | TODO |

## Bảng PAYMENT

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | TODO | PK | TODO |
| order_id | TODO | FK → ORDERS.id | TODO |
| phuong_thuc | TODO | COD / chuyển khoản / cổng thanh toán | TODO |
| so_tien | TODO | TODO | TODO |
| trang_thai | TODO | Giá trị cố định (xem quy ước) | TODO |
| ma_giao_dich | TODO | TODO | TODO |
| thoi_gian | TODO | TODO | TODO |

## Bảng SHIPMENT

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
| --- | --- | --- | --- |
| id | TODO | PK | TODO |
| order_id | TODO | FK → ORDERS.id | TODO |
| carrier_id | TODO | FK → CARRIER.id | TODO |
| ma_van_don | TODO | TODO | TODO |
| phi | TODO | TODO | TODO |
| trang_thai | TODO | Giá trị cố định (xem quy ước) | TODO |
| ngay_giao | TODO | TODO | TODO |

## Danh sách ràng buộc toàn cục

- Tồn kho ≥ 0 (PRODUCT.ton_kho).
- Đơn giá lưu ở ORDER_ITEM (giá tại thời điểm mua, BR3).
- Các cột trạng thái dùng danh sách giá trị cố định (trạng thái đơn, thanh toán, vận đơn).
- Email người dùng là duy nhất (USER.email).
- TODO: bổ sung các ràng buộc khác (quan hệ 1–1, hạn dùng mã giảm giá, số lượt dùng…)
