# SD1 – Thêm vào giỏ (UC10)

> **Phụ trách:** Huỳnh Nguyễn Quốc Việt  
> **Trạng thái:** 🟠 Chờ duyệt  
> **Hạn:** —

---

## 1. Sơ đồ tuần tự (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor Khach as Khách hàng<br/>(Guest/Customer)
    participant UI as :CartUI<br/>(Màn hình C3/C2)
    participant Ctrl as :CartController
    participant ProdSvc as :ProductService
    participant Prod as :Product
    participant Cart as :Cart
    participant DB as :Database

    Khach->>UI: Chọn số lượng (qty) & Nhấn "Thêm vào giỏ"
    activate UI
    UI->>Ctrl: addToCart(productId, qty)
    activate Ctrl

    %% 1. Kiểm tra tồn kho sản phẩm
    Ctrl->>ProdSvc: checkAvailability(productId, qty)
    activate ProdSvc
    ProdSvc->>DB: SELECT ton_kho, trang_thai FROM PRODUCT WHERE id = productId
    activate DB
    DB-->>ProdSvc: productRecord (tonKho, trangThai)
    deactivate DB

    ProdSvc->>Prod: checkStock(currentInCartQty + qty)
    activate Prod
    Note over Prod: Kiểm tra BR2:<br/>(SL giỏ + SL thêm) <= ton_kho
    Prod-->>ProdSvc: isAvailable (true / false)
    deactivate Prod

    %% 2. Rẽ nhánh theo kết quả kiểm tra tồn kho
    alt Đủ hàng trong kho (isAvailable == true)
        ProdSvc-->>Ctrl: stockValid (true)
        deactivate ProdSvc

        %% Thêm sản phẩm vào giỏ
        Ctrl->>Cart: addItem(productId, qty)
        activate Cart
        Cart->>DB: SELECT id, so_luong FROM CART_ITEM WHERE cart_id = ? AND product_id = ?
        activate DB
        DB-->>Cart: itemRecord (hoặc null nếu chưa có)
        deactivate DB

        alt Sản phẩm đã tồn tại trong giỏ
            Cart->>DB: UPDATE CART_ITEM SET so_luong = so_luong + qty WHERE id = ?
            activate DB
            DB-->>Cart: updateSuccess
            deactivate DB
        else Sản phẩm chưa có trong giỏ
            Cart->>DB: INSERT INTO CART_ITEM (cart_id, product_id, so_luong) VALUES (?, ?, ?)
            activate DB
            DB-->>Cart: insertSuccess
            deactivate DB
        end

        Cart-->>Ctrl: cartSummary (totalItems, subtotal)
        deactivate Cart

        Ctrl-->>UI: 200 OK (Thành công, cartSummary)
        deactivate Ctrl
        UI-->>Khach: Cập nhật Cart Badge & Hiển thị Toast "Thêm vào giỏ thành công!"
        deactivate UI

    else Hết hàng hoặc Vượt quá tồn kho (isAvailable == false)
        ProdSvc-->>Ctrl: stockError ("Số lượng yêu cầu vượt quá tồn kho khả dụng")
        deactivate ProdSvc

        Ctrl-->>UI: 400 Bad Request (Thông báo lỗi tồn kho)
        deactivate Ctrl
        UI-->>Khach: Hiển thị cảnh báo lỗi ("Không đủ số lượng trong kho")
        deactivate UI
    end
```

---

## 2. Giải thích sơ đồ (Thuyết minh kỹ thuật)

1. **Mục đích:** Sơ đồ mô hình hóa tương tác theo thời gian khi người dùng (Khách vãng lai hoặc Khách đã đăng nhập) thực hiện thêm một món đồ chơi vào giỏ hàng cá nhân từ màn hình Chi tiết sản phẩm (`C3`) hoặc Danh mục (`C2`).
2. **Kiểm tra tồn kho 2 lớp (`BR2`):** Hệ thống không ghi trực tiếp vào giỏ mà bắt buộc đối soát số lượng tồn kho khả dụng qua phương thức `Product.checkStock()` với tổng số lượng yêu cầu bằng `(Số lượng đã có trong giỏ + Số lượng thêm mới)`.
3. **Phân nhánh xử lý (`alt`):**
   - **Nhánh thành công (`isAvailable == true`):** Gọi `Cart.addItem()`, tự động phát hiện sản phẩm đã có sẵn để cập nhật tăng số lượng (`UPDATE CART_ITEM`) hoặc thêm mới dòng mới (`INSERT INTO CART_ITEM`). Tồn kho `PRODUCT.ton_kho` **chưa bị trừ** ở bước này theo quy tắc nghiệp vụ `BR2`.
   - **Nhánh thất bại (`isAvailable == false`):** Trả về mã lỗi `400 Bad Request`, giao diện hiển thị toast cảnh báo hết hàng/vượt quá tồn kho và giữ nguyên trạng thái giỏ hàng.
4. **Phản hồi thời gian thực:** Khi thành công, thanh điều hướng Header cập nhật số đếm trên biểu tượng giỏ hàng (Cart Badge) và thông báo Toast cho phép khách *"Tiếp tục mua hàng"* hoặc chuyển sang *"Xem giỏ hàng (`C4`)"*.

---

## 3. Đối chiếu với Class Diagram của Quang

Bảng đối chiếu đảm bảo sự thống nhất 100% giữa thông điệp trong sơ đồ tuần tự và phương thức trong mô hình lớp [03_class_diagram.md](../03_class_diagram.md):

| Thông điệp trong sơ đồ | Lớp tiếp nhận (Class) | Phương thức trong Class Diagram | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- | :--- |
| `checkStock(requestedQty)` | `Product` | `+checkStock(): boolean` | Kiểm tra số lượng đồ chơi yêu cầu có nhỏ hơn hoặc bằng số lượng tồn kho khả dụng (`ton_kho`) hay không (`BR2`). |
| `addItem(productId, qty)` | `Cart` | `+addItem()` | Thêm mới một mặt hàng hoặc cộng dồn số lượng vào đối tượng giỏ hàng và danh sách `CartItem`. |
| `productRecord (tonKho, trangThai)` | `Product` | Thuộc tính `tonKho: int`, `gia: decimal` | Trích xuất thông tin tồn kho và trạng thái kinh doanh của sản phẩm từ CSDL. |
| `cartSummary (totalItems, subtotal)` | `Cart` | Thuộc tính `id: int` | Tổng hợp số lượng món đồ và giá trị tạm tính của giỏ hàng để phản hồi về giao diện người dùng. |
