# Class Diagram (8 lớp lõi)

> **Phụ trách:** Trần Vinh Quang  
> **Trạng thái:** 🔴 Chưa làm  
> **Hạn:** —

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

TODO (chèn hình)

## Danh sách lớp và ánh xạ sang bảng

| Lớp | Thuộc tính chính | Phương thức | Ánh xạ bảng ERD |
| --- | --- | --- | --- |
| User | TODO | TODO | USER, ADDRESS |
| Product | TODO | TODO | PRODUCT, PRODUCT_IMAGE |
| Category | TODO | TODO | CATEGORY |
| Cart – CartItem | TODO | TODO | CART, CART_ITEM |
| Order – OrderItem | TODO | TODO | ORDERS, ORDER_ITEM |
| Payment | TODO | TODO | PAYMENT |
| Shipment | TODO | TODO | SHIPMENT, CARRIER |
| Coupon | TODO | TODO | COUPON |

## Giải thích

- Quan hệ Association / Aggregation / Composition / Generalization đã dùng và lý do: TODO
- Cách User thể hiện vai trò (kế thừa Customer/Staff/Admin hay thuộc tính `vai_tro`) và vì sao: TODO
