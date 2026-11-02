# Class Diagram (8 lớp lõi)

> **Phụ trách:** Trần Vinh Quang  
> **Trạng thái:** 🟠 Chờ duyệt
> **Hạn:** 

<!-- huong-dan:start -->
> 💡 Vẽ **Class Diagram rút gọn 8 lớp lõi**: User, Product, Category, Cart–CartItem, Order–OrderItem, Payment, Shipment, Coupon. Mỗi lớp có thuộc tính + 2–3 phương thức và multiplicity. Phương thức đã chốt trong kế hoạch: `Cart.addItem()`, `Order.calculateTotal()`, `Order.updateStatus()`, `Shipment.track()`.
> Bản nháp Mermaid dưới đây chỉ là **điểm xuất phát** (GitHub hiển thị được); hãy chỉnh rồi vẽ lại bằng draw.io và xuất PNG vào `hinh/`. Tên phương thức phải thống nhất với Việt (Sequence).

```mermaid
classDiagram
    class User {
        +int id
        +String hoTen
        +String email
        +String vaiTro
        +login()
        +logout()
    }
    class Category {
        +int id
        +String ten
    }
    class Product {
        +int id
        +String ten
        +decimal gia
        +int tonKho
        +checkStock()
    }
    class Cart {
        +int id
        +addItem()
        +removeItem()
    }
    class CartItem {
        +int soLuong
    }
    class Order {
        +int id
        +String trangThai
        +decimal tongTien
        +calculateTotal()
        +updateStatus()
    }
    class OrderItem {
        +int soLuong
        +decimal donGia
    }
    class Payment {
        +String phuongThuc
        +String trangThai
        +pay()
    }
    class Shipment {
        +String maVanDon
        +String trangThai
        +track()
    }
    class Coupon {
        +String ma
        +isValid()
    }
    User "1" -- "1" Cart
    User "1" -- "*" Order
    Category "1" -- "*" Product
    Cart "1" *-- "*" CartItem
    CartItem "*" --> "1" Product
    Order "1" *-- "*" OrderItem
    OrderItem "*" --> "1" Product
    Order "1" -- "1" Payment
    Order "1" -- "0..1" Shipment
    Coupon "0..1" -- "*" Order
```
<!-- huong-dan:end -->

## Sơ đồ

```mermaid
classDiagram
    direction TB

    class User {
        +int id
        +String hoTen
        +String email
        +String sdt
        +String vaiTro
        +login() boolean
        +logout() void
    }
    class Category {
        +int id
        +String ten
        +getProducts() List
    }
    class Product {
        +int id
        +String ten
        +decimal gia
        +int tonKho
        +checkStock(soLuong) boolean
        +updateStock(delta) void
    }
    class Cart["Cart-CartItem"] {
        +int id
        +int soLuong
        +addItem(sp, soLuong) void
        +removeItem(sp) void
        +calculateSubtotal() decimal
    }
    class Order["Order-OrderItem"] {
        +int id
        +String trangThai
        +decimal tongTien
        +decimal phiShip
        +decimal giamGia
        +datetime ngayTao
        +int soLuong
        +decimal donGia
        +calculateTotal() decimal
        +updateStatus(trangThai) void
        +cancel() void
    }
    class Payment {
        +int id
        +String phuongThuc
        +decimal soTien
        +String trangThai
        +pay() boolean
        +refund() void
    }
    class Shipment {
        +int id
        +String maVanDon
        +decimal phi
        +String trangThai
        +createShipment() void
        +track() String
    }
    class Coupon {
        +int id
        +String ma
        +decimal giaTri
        +date hanDung
        +isValid(tongTien) boolean
        +apply(tongTien) decimal
    }

    Category "1" --> "0..*" Product : chứa
    User "1" --> "1" Cart : sở hữu
    User "1" --> "0..*" Order : đặt
    Cart "0..*" --> "0..*" Product : gồm (soLuong)
    Order "0..*" --> "1..*" Product : gồm (soLuong, donGia)
    Order "1" --> "1" Payment : thanh toán
    Order "1" --> "0..1" Shipment : giao hàng
    Coupon "0..1" --> "0..*" Order : áp dụng cho
```
## Danh sách lớp và ánh xạ sang bảng

| Lớp | Thuộc tính chính | Phương thức | Ánh xạ bảng ERD |
| --- | --- | --- | --- |
| User | id, hoTen, email, sdt, vaiTro (Guest/Customer/Staff/Admin) | login(), logout() | USER, ADDRESS |
| Product | id, ten, gia, tonKho | checkStock(soLuong), updateStock(delta) | PRODUCT, PRODUCT_IMAGE |
| Category | id, ten | getProducts() | CATEGORY |
| Cart – CartItem | Cart: id<br>CartItem: soLuong | addItem(sp, soLuong), removeItem(sp), calculateSubtotal() | CART, CART_ITEM |
| Order – OrderItem | Order: id, trangThai, tongTien, phiShip, giamGia, ngayTao<br>OrderItem: soLuong, donGia (giá tại thời điểm mua) | calculateTotal(), updateStatus(trangThai), cancel() | ORDERS, ORDER_ITEM |
| Payment | id, phuongThuc (COD / chuyển khoản / cổng mô phỏng), soTien, trangThai | pay(), refund() | PAYMENT |
| Shipment | id, maVanDon, phi, trangThai | createShipment(), track() | SHIPMENT, CARRIER |
| Coupon | id, ma, giaTri, hanDung | isValid(tongTien), apply(tongTien) | COUPON |


## Giải thích

- Quan hệ Association / Aggregation / Composition / Generalization đã dùng và lý do:
  - **Association** (cả 8 quan hệ): các lớp chỉ cần biết đến nhau, không ràng buộc vòng đời. Có kèm multiplicity, ví dụ Order – Shipment là `1` – `0..1` (đơn chưa xác nhận hoặc đã hủy thì chưa có vận đơn), Coupon – Order là `0..1` – `0..*` (mỗi đơn dùng tối đa một mã).
  - **Cart – Product, Order – Product** là association nhiều–nhiều mang `soLuong` (và `donGia` = giá tại thời điểm mua). CartItem và OrderItem được gộp vào Cart và Order cho đủ 8 lớp; về bản chất là composition và sẽ tách thành bảng CART_ITEM, ORDER_ITEM ở ERD.
  - **Aggregation, Generalization:** không dùng vì không có quan hệ "bộ phận – tổng thể tồn tại độc lập" hay lớp con nào cần thêm thuộc tính riêng.

- Cách User thể hiện vai trò (kế thừa Customer/Staff/Admin hay thuộc tính `vai_tro`) và vì sao:
  - Dùng **thuộc tính `vaiTro`** (Guest / Customer / Staff / Admin), không tách lớp con.
  - Vì ba loại tài khoản có dữ liệu và hành vi giống nhau, chỉ khác quyền truy cập; khớp với bảng USER duy nhất trong ERD (cột `vai_tro`); giữ đúng phạm vi 8 lớp lõi; và dễ thêm vai trò mới mà không đổi cấu trúc. Quan hệ kế thừa giữa các actor đã thể hiện ở Use Case Diagram.
