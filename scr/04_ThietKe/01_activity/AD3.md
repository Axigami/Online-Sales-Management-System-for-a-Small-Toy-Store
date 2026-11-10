# AD3 – Thêm vào giỏ và quản lý giỏ hàng

> **Phụ trách:** Tô Đoàn Khánh Nguyên  
> **Trạng thái:** 🟠 Chờ duyệt  
> **Hạn:** 2026-11-05  
> **File thiết kế Draw.io:** [activity_diagrams.drawio](./activity_diagrams.drawio) (Tab AD3)  

Tài liệu mô hình hóa luồng nghiệp vụ **Thêm sản phẩm vào giỏ hàng và Quản lý giỏ hàng (Add to Cart & Cart Management Flow)** thông qua sơ đồ hoạt động phân làn (Activity Diagram with Swimlanes). Luồng này thể hiện tính nhất quán chặt chẽ với quy tắc nghiệp vụ kiểm tra tồn kho thời gian thực (**BR2**) và cơ chế lưu trữ giỏ tạm cũng như tự động gộp giỏ hàng khi đăng nhập (**BR1**).

* **Ca sử dụng liên quan:** `UC10 (Thêm sản phẩm vào giỏ)`, `UC11 (Quản lý giỏ hàng: Xem/Sửa/Xóa)`, `UC02 (Đăng nhập tài khoản)`.
* **Phân chia 2 làn bơi (Swimlanes):**
  1. `Khách hàng / Khách vãng lai (Customer / Guest)`: Tác nhân thực hiện các tương tác chọn món, điều chỉnh số lượng, xóa sản phẩm và tiến hành đặt đơn.
  2. `Hệ thống (System)`: Xử lý logic truy vấn tồn kho tức thời, lưu trữ phiên Session/LocalStorage hoặc CSDL bảng `CART`, `CART_ITEM`, tính toán lại tổng tiền tạm tính và tự động gộp giỏ hàng.

---

## 1. Sơ đồ hoạt động AD3 (Activity Diagram)

### Sơ đồ trực quan

![Sơ đồ hoạt động AD3 – Thêm vào giỏ và quản lý giỏ hàng](./Hình%20activity%20diagram/Thêm%20vào%20giỏ%20hàng%20và%20Quản%20lý%20giỏ%20hàng.png)

---

## 2. Giải thích sơ đồ hoạt động (Thuyết minh kỹ thuật)

1. **Kiểm tra tồn kho thời gian thực (Real-time Inventory Validation - BR2):**
   * Khi khách bấm *Thêm vào giỏ* hoặc nhấn nút *Tăng số lượng (+)* trong trang giỏ hàng, hệ thống không chỉ kiểm tra cục bộ mà ngay lập tức thực hiện truy vấn số lượng tồn kho khả dụng (`stock - reserved`) trong bảng `PRODUCT`.
   * Nếu người dùng yêu cầu vượt quá số lượng hàng thực tế tại cửa hàng, hệ thống chặn ngay tại chỗ và phát cảnh báo kèm số lượng tối đa có thể mua, ngăn chặn hiện tượng tạo đơn ảo không có hàng giao.

2. **Cơ chế giỏ hàng 2 tầng và Tự động gộp giỏ hàng (Auto-merge Cart - BR1):**
   * **Khách vãng lai (Guest):** Thông tin giỏ hàng được duy trì trong Session / LocalStorage của trình duyệt, cho phép người dùng thoải mái mua sắm mà không bắt buộc tạo tài khoản ngay từ đầu (giảm thiểu rào cản mua sắm - Drop-off rate).
   * **Khách hàng (Customer):** Khi khách đăng nhập (`UC02`), hệ thống tự động kích hoạt thuật toán đồng bộ (Merge logic): duyệt toàn bộ các món trong Session và thêm vào bảng `CART_ITEM` trong CSDL. Nếu sản phẩm đã tồn tại trong giỏ CSDL thì cộng dồn số lượng (có kiểm tra trần tồn kho) và xóa sạch giỏ tạm ở client.

3. **Điều hướng mạch lạc và Điểm kết nối với sơ đồ AD1:**
   * Khi người dùng quyết định mua và bấm *Tiến hành đặt hàng*, hệ thống thực hiện chốt kiểm tra đăng nhập bắt buộc (**BR1**).
   * Sau khi tài khoản và giỏ hàng CSDL đã sẵn sàng, hệ thống điều hướng chuyển tiếp toàn bộ danh mục hàng sang quy trình **AD1 (Đặt hàng và thanh toán)** để tính cước vận chuyển, áp mã giảm giá và thanh toán.

