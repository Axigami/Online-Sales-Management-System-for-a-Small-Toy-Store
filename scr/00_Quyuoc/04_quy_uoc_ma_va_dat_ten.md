# Quy ước mã và đặt tên

Script `scripts/check.py` kiểm tra tự động các quy ước dưới đây. Dòng nào cố ý vi phạm (ví dụ ví dụ minh họa) thì thêm `<!-- skip-check -->` cuối dòng.

## 1. Mã thống nhất

| Loại | Quy ước | Ví dụ |
| --- | --- | --- |
| Use Case | `UC` + 2 chữ số, từ UC01 đến UC25 | UC13 |
| Yêu cầu chức năng | `FR` + 2 chữ số trở lên | FR01, FR27 |
| Yêu cầu phi chức năng | `NFR` + 2 chữ số | NFR03 |
| Activity Diagram | AD1–AD2 | AD1 |
| Sequence Diagram | SD1–SD4 | SD2 |
| Màn hình khách hàng | C1–C8 | C5 |
| Màn hình admin | A1–A5 | A4 |
| Quy tắc nghiệp vụ | BR1–BR6 | BR2 |

## 2. Tên bảng (14 bảng, viết HOA, khớp ERD)

`USER`, `ADDRESS`, `CATEGORY`, `PRODUCT`, `PRODUCT_IMAGE`, `CART`, `CART_ITEM`, `ORDERS`, `ORDER_ITEM`, `PAYMENT`, `SHIPMENT`, `CARRIER`, `COUPON`, `INVENTORY_LOG`

- Bảng đơn hàng đặt tên **`ORDERS`** (số nhiều) vì `ORDER` là từ khóa của SQL.
- `USER` cũng là từ khóa trong một số hệ quản trị (vd PostgreSQL). Nếu Quang đổi tên (vd `APP_USER`) thì phải báo nhóm trưởng để cập nhật quy ước này.
- Dùng đúng dạng số ít như trên cho các bảng còn lại (không viết `PRODUCTS`, `CART_ITEMS`, `ORDERITEM`…).
- Tên cột: chữ thường, không dấu, nối bằng `_` (ví dụ `don_gia`, `ma_van_don`). Quang chốt danh sách cột cuối cùng.

## 3. Đặt tên file và thư mục

- Thư mục `docs/` giữ đúng 6 nhóm: `01_KhaoSat`, `02_YeuCau`, `03_UseCase`, `04_ThietKe`, `05_UI`, `06_BaoCao`.
- Tên file: chữ thường, không dấu, nối bằng `_`, bắt đầu bằng số thứ tự hoặc mã (`01_...`, `UC13.md`, `SD2.md`).
- Hình: `hinh/<ma>_<ten>.png` + `hinh/<ma>_<ten>.drawio`.

## 4. Đầu mỗi file sản phẩm

```markdown
> **Phụ trách:** (đúng họ tên trong team.json)
> **Trạng thái:** 🔴 Chưa làm | 🟡 Đang làm | 🟠 Chờ duyệt | 🔵 Cần sửa | ✅ Đã duyệt
> **Hạn:** YYYY-MM-DD hoặc —
```

Quy tắc kiểm tra tự động:

- File ở trạng thái 🟠 hoặc ✅ **không được còn** chữ `TODO` hay khối `huong-dan`.
- Người không phải nhóm trưởng không được đặt ✅ Đã duyệt trong PR.
