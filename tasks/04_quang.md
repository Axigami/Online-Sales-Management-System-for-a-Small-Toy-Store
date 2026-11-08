# Trần Vinh Quang – 52300248 (Thiết kế dữ liệu)

**Phụ trách:** Class Diagram • ERD • Từ điển dữ liệu

> Tick `[x]` khi xong từng mục (file này thuộc phạm vi của bạn). Trạng thái chính thức của từng sản phẩm nằm ở **đầu file sản phẩm**.

## Các bước làm

- [x] Lấy **bảng 14 thực thể** (trong `04_erd.md`) làm gốc; bổ sung kiểu dữ liệu, khóa chính (PK), khóa ngoại (FK), ràng buộc
- [x] Vẽ **Class Diagram rút gọn 8 lớp lõi:** User (kế thừa Customer/Staff/Admin hoặc dùng thuộc tính vai_trò), Product, Category, Cart–CartItem, Order–OrderItem, Payment, Shipment, Coupon. Mỗi lớp có thuộc tính + 2–3 phương thức (vd Cart.addItem(), Order.calculateTotal(), Order.updateStatus(), Shipment.track()) và multiplicity
- [x] Vẽ **ERD đầy đủ 14 bảng**, ghi rõ khóa và cardinality
- [x] Viết **từ điển dữ liệu** cho 6 bảng chính: PRODUCT, CART_ITEM, ORDERS, ORDER_ITEM, PAYMENT, SHIPMENT (tên cột – kiểu – ràng buộc – mô tả)
- [x] Ghi ràng buộc: tồn kho ≥ 0; đơn giá lưu ở ORDER_ITEM; trạng thái dùng danh sách giá trị cố định; email duy nhất
- [x] Thống nhất tên lớp/phương thức với Việt; gửi ERD cho Phương và Tâm khi cần trường dữ liệu cho màn hình
- [x] (Tùy chọn) Xuất script SQL tạo bảng → `docs/04_ThietKe/sql/` (tạo thêm thư mục và báo nhóm trưởng để thêm vào phạm vi)

## Output cuối cùng

- [x] Class Diagram (8 lớp) có thuộc tính, phương thức, multiplicity
- [x] ERD đầy đủ 14 bảng (PK, FK, cardinality)
- [x] Từ điển dữ liệu 6 bảng chính + danh sách ràng buộc

## Phần của mình nối với ai (trả lời câu 3 khi báo cáo)

- **Việt:** tên phương thức trong Class Diagram phải khớp thông điệp trong SD1–SD4.
- **Phương, Tâm:** cung cấp trường dữ liệu để vẽ màn hình.
- **Nguyên:** nhận FR/UC để chốt thực thể.

## Khái niệm cần giải thích được

- **Entity – Attribute – Relationship:** thực thể – thuộc tính – mối quan hệ
- **Cardinality (1–1, 1–N, N–N):** bội số quan hệ; quan hệ N–N phải tách bảng trung gian (ORDERS – PRODUCT → ORDER_ITEM)
- **PK / FK:** khóa chính (định danh duy nhất) / khóa ngoại (trỏ sang bảng khác)
- **Chuẩn hóa (1NF–3NF) mức cơ bản:** tránh lặp dữ liệu, mỗi thông tin chỉ lưu một nơi
- **Class Diagram:** lớp, thuộc tính, phương thức, multiplicity; Association, Aggregation, Composition (Order – OrderItem), Generalization (User → Customer)
- **Class vs ERD:** Class = đối tượng + hành vi (thiết kế phần mềm); ERD = dữ liệu lưu trữ (thiết kế CSDL)

## Trọng tâm khi báo cáo

**Mỗi người phải trả lời được 3 câu:** (1) Phần mình làm gì? (2) Vì sao làm như vậy? (3) Phần mình nối với phần của ai?

- Giải thích 3 quyết định thiết kế: (1) tách ORDER_ITEM, (2) lưu đơn giá tại thời điểm mua, (3) tách PAYMENT và SHIPMENT khỏi ORDERS.
- Chỉ ra trên ERD luồng Giỏ hàng → Đơn hàng → Vận chuyển nối nhau thế nào.
- Class Diagram và ERD ánh xạ với nhau ra sao (lớp ↔ bảng).
