# SD2 – Đặt hàng và Thanh toán online (UC13 + UC14)

> **Phụ trách:** Huỳnh Nguyễn Quốc Việt
> **Trạng thái:** 🟠 Chờ duyệt
> **Hạn:** —

> [!NOTE]
> File SD2 được chia thành **2 sơ đồ tuần tự con (SD2A & SD2B)** để dễ trình bày và tối ưu trực quan, nhưng vẫn nằm trong cùng 1 file theo đúng quy ước `SD1–SD4` của nhóm:
> - **SD2A:** Đặt hàng & Thanh toán khi nhận hàng / Chuyển khoản (UC13).
> - **SD2B:** Thanh toán trực tuyến qua Cổng VNPAY/MoMo (UC14 «extend»).

---

## Sơ đồ 2A – Đặt hàng & Thanh toán khi nhận hàng / Chuyển khoản (UC13)

**Phạm vi:** Khách hàng checkout từ giỏ hàng (C4) → kiểm tra tồn kho vòng 1 (Real-time) → hiển thị Checkout (C5) → tính phí ship (UC20) → áp mã coupon (UC12) → chọn COD/Chuyển khoản → thực thi ACID Transaction (tạo đơn, chốt giá cứng, trừ kho, xóa giỏ) → hiển thị thành công (C6).

```mermaid
sequenceDiagram
    autonumber
    actor Khach as Khach hang<br/>(Customer)
    participant UI as :CheckoutUI<br/>(Man hinh C5/C6)
    participant Ctrl as :OrderController
    participant OrdSvc as :OrderService
    participant DB as :Database

    %% BƯỚC 1: Khách hàng bắt đầu checkout từ giỏ C4
    Khach->>UI: Nhan "Tien hanh dat hang" (tu man hinh C4)
    activate UI
    UI->>Ctrl: checkout(cartId, userId)
    activate Ctrl

    %% BƯỚC 2: Kiểm tra tồn kho vòng 1 – Real-time check (BR2)
    Ctrl->>DB: queryCartItems(cartId)
    activate DB
    Note over DB: SELECT p.ton_kho, ci.so_luong<br/>FROM CART_ITEM ci JOIN PRODUCT p<br/>WHERE ci.cart_id = ?
    DB-->>Ctrl: cartItems[ ] (soLuong, tonKho, gia)
    deactivate DB

    Ctrl->>OrdSvc: checkStock(cartItems)
    activate OrdSvc
    Note over OrdSvc: Goi Product.checkStock(soLuong)<br/>cho tung san pham trong gio (BR2)
    OrdSvc-->>Ctrl: stockResult (isAvailable, outOfStockItems)
    deactivate OrdSvc

    alt Gio hang co san pham het hang (isAvailable == false)

        Ctrl-->>UI: 409 Conflict (outOfStockItems)
        UI-->>Khach: Canh bao "San pham [X] chi con [n] cai – vui long chinh lai gio"
        Note over UI: Chuyen Khach ve man hinh C4 (Gio hang)<br/>Ket thuc luong – 3-E1 trong UC13

    else Toan bo san pham con du hang (isAvailable == true)

        %% BƯỚC 3: Hiển thị màn hình Checkout C5
        Ctrl-->>UI: 200 OK (checkoutData)
        UI-->>Khach: Hien thi man hinh Checkout C5 (dia chi, phi ship, coupon, PTTT)

        %% BƯỚC 4: Tính phí ship (UC20 «include»)
        Khach->>UI: Chon/nhap dia chi giao hang
        UI->>Ctrl: calculateShipping(addressId, cartItems)
        Ctrl->>OrdSvc: calculateShipping(addressId, cartItems)
        activate OrdSvc
        Note over OrdSvc: Tinh phi theo khu vuc & khoi luong (UC20)<br/>Neu tong tien hang >= 500.000d thi phiShip = 0 (BR4)
        OrdSvc-->>Ctrl: shippingFee (phiShip)
        deactivate OrdSvc
        Ctrl-->>UI: shippingFee (phiShip)
        UI-->>Khach: Cap nhat phi van chuyen tren C5

        %% BƯỚC 5: Áp mã giảm giá (UC12 «extend» – tùy chọn)
        opt Khach nhap ma giam gia
            Khach->>UI: Nhap ma coupon & nhan "Ap dung"
            UI->>Ctrl: applyCoupon(couponCode, subTotal)
            Ctrl->>OrdSvc: validateCoupon(couponCode, subTotal)
            activate OrdSvc
            Note over OrdSvc: Coupon.isValid(tongTien): boolean (BR5)<br/>Neu hop le -> Coupon.apply(tongTien): decimal
            OrdSvc-->>Ctrl: couponResult (giamGia, tongTienSauGiam)
            deactivate OrdSvc
            Ctrl-->>UI: couponResult (giamGia, tongTienSauGiam)
            UI-->>Khach: Cap nhat tong tien sau giam gia tren C5
        end

        %% BƯỚC 6: Khách xem hóa đơn & xác nhận đặt hàng
        Ctrl->>OrdSvc: calculateTotal(subTotal, phiShip, giamGia)
        activate OrdSvc
        OrdSvc-->>Ctrl: tongTien (decimal)
        deactivate OrdSvc
        Ctrl-->>UI: invoiceSummary (tongTien, phiShip, giamGia)
        UI-->>Khach: Hien thi bang tom tat hoa don tren C5

        Khach->>UI: Chon PTTT (COD / Chuyen khoan) & nhan "Dat hang ngay"
        UI->>Ctrl: placeOrder(cartId, addressId, shippingFee, giamGia, phuongThuc)

        %% BƯỚC 7: ACID Transaction – Tạo đơn hàng COD/CK (BR2, BR3)
        Ctrl->>DB: beginTransaction()
        activate DB
        Ctrl->>DB: insertOrder(userId, tongTien, phiShip, giamGia, trangThai, trangThaiTT)
        Note over DB: ORDERS(trangThai='Cho xac nhan',<br/>trangThaiTT='Chua thanh toan')
        DB-->>Ctrl: orderId

        Ctrl->>DB: insertOrderItems(orderId, cartItems, donGia)
        Note over DB: ORDER_ITEM(donGia = gia chot cung tai thoi diem mua - BR3)
        DB-->>Ctrl: orderItemsCreated

        Ctrl->>DB: updateStock(productIds, soLuong)
        Note over DB: UPDATE PRODUCT SET ton_kho = ton_kho - soLuong (BR2)
        DB-->>Ctrl: stockUpdated

        Ctrl->>DB: removeCartItems(cartId)
        Note over DB: DELETE FROM CART_ITEM WHERE cart_id = ?
        DB-->>Ctrl: cartCleared

        Ctrl->>DB: commitTransaction()
        deactivate DB

        Ctrl->>OrdSvc: updateStatus(orderId, "Cho xac nhan")
        activate OrdSvc
        OrdSvc-->>Ctrl: statusUpdated
        deactivate OrdSvc

        Ctrl-->>UI: 201 Created (orderId)
        UI-->>Khach: Man hinh C6 - "Dat hang thanh cong!" (ma don, gui email xac nhan)
        Note over UI: He thong gui email xac nhan don hang den KH<br/>(SYS1 trong AD1)

    end
    deactivate Ctrl
    deactivate UI
```

---

## Sơ đồ 2B – Thanh toán trực tuyến qua Cổng VNPAY/MoMo (UC14 «extend»)

**Phạm vi:** Khách hàng chọn phương thức Online tại màn hình C5 → hệ thống khởi tạo giao dịch bảo mật HMAC-SHA512 → chuyển hướng sang Cổng thanh toán → khách xác thực OTP/QR → Cổng gọi IPN Webhook về hệ thống → xác thực chữ ký & Idempotency → rẽ nhánh xử lý thành công hoặc thất bại.

```mermaid
sequenceDiagram
    autonumber
    actor Khach as Khach hang<br/>(Customer)
    participant UI as :CheckoutUI<br/>(Man hinh C5/C6)
    participant Ctrl as :OrderController
    participant OrdSvc as :OrderService
    participant PaySvc as :PaymentService
    participant GW as :PaymentGateway<br/>(VNPAY/MoMo)
    participant DB as :Database

    %% BƯỚC 1: Khách hàng chọn thanh toán Online tại C5
    Khach->>UI: Chon "Thanh toan Online" (VNPAY/MoMo) & nhan "Thanh toan"
    activate UI
    UI->>Ctrl: placeOnlineOrder(cartId, addressId, shippingFee, giamGia, phuongThuc)
    activate Ctrl

    %% BƯỚC 2: Khởi tạo phiên thanh toán online (HMAC-SHA512)
    Ctrl->>PaySvc: initiatePayment(tongTien, phuongThuc)
    activate PaySvc
    Note over PaySvc: Dong goi giao dich<br/>+ ky HMAC-SHA512 (NFR bao mat)
    PaySvc->>GW: createPaymentSession(amount, refId, hmacSignature)
    activate GW
    GW-->>PaySvc: paymentUrl
    deactivate GW
    PaySvc-->>Ctrl: paymentUrl
    deactivate PaySvc
    Ctrl-->>UI: redirect(paymentUrl)
    UI-->>Khach: Chuyen huong sang giao dien cong thanh toan

    %% BƯỚC 3: Khách hàng xác thực trên cổng thanh toán
    Khach->>GW: Thuc hien xac thuc (OTP / Quet QR)
    activate GW

    %% BƯỚC 4: Gateway gửi IPN Webhook về PaymentService
    GW-->>PaySvc: gatewayResult (status, maGiaoDich, hmacSignature)
    deactivate GW
    activate PaySvc
    Note over PaySvc: Kiem tra HMAC-SHA512<br/>+ Idempotency (maGiaoDich chua xu ly)
    PaySvc-->>Ctrl: ipnResult (status, maGiaoDich)
    deactivate PaySvc

    %% BƯỚC 5: Rẽ nhánh theo kết quả thanh toán
    alt Giao dich thanh cong (status=SUCCESS)

        %% ACID Transaction – tạo đơn hàng online thành công
        Ctrl->>DB: beginTransaction()
        activate DB
        Ctrl->>DB: insertOrder(userId, tongTien, phiShip, giamGia, trangThai, trangThaiTT)
        Note over DB: ORDERS(trangThai='Cho xac nhan',<br/>trangThaiTT='Da thanh toan')
        DB-->>Ctrl: orderId

        Ctrl->>DB: insertOrderItems(orderId, cartItems, donGia)
        Note over DB: ORDER_ITEM(donGia = gia chot cung tai thoi diem mua - BR3)
        DB-->>Ctrl: orderItemsCreated

        Ctrl->>DB: updateStock(productIds, soLuong)
        Note over DB: UPDATE PRODUCT SET ton_kho = ton_kho - soLuong (BR2)
        DB-->>Ctrl: stockUpdated

        Ctrl->>DB: insertPayment(orderId, phuongThuc, soTien, trangThai, maGiaoDich)
        Note over DB: PAYMENT(trangThai='Da thanh toan')
        DB-->>Ctrl: paymentId

        Ctrl->>DB: removeCartItems(cartId)
        Note over DB: DELETE FROM CART_ITEM WHERE cart_id = ?
        DB-->>Ctrl: cartCleared

        Ctrl->>DB: commitTransaction()
        deactivate DB

        Ctrl->>OrdSvc: updateStatus(orderId, "Cho xac nhan")
        activate OrdSvc
        OrdSvc-->>Ctrl: statusUpdated
        deactivate OrdSvc

        Ctrl-->>UI: 201 Created (orderId, maGiaoDich)
        UI-->>Khach: Man hinh C6 - "Dat hang & Thanh toan thanh cong!" (ma don, ma giao dich, gui email xac nhan)
        Note over UI: He thong gui email xac nhan don hang den KH<br/>(SYS1 trong AD1)

    else Giao dich that bai hoac bi huy (status=FAILED / CANCEL)

        activate PaySvc
        PaySvc->>DB: logPaymentFailure(phuongThuc, trangThai)
        activate DB
        Note over DB: PAYMENT(trangThai='That bai')
        DB-->>PaySvc: paymentLogId
        deactivate DB
        PaySvc-->>Ctrl: ipnResult (valid=false, errorCode)
        deactivate PaySvc
        Ctrl-->>UI: 402 Payment Required (errorCode, message)
        UI-->>Khach: Thong bao loi thanh toan tren C5 - "Thu lai" hoac doi sang COD (7a-E1 trong UC13)

    end
    deactivate Ctrl
    deactivate UI
```

---

## Giải thích sơ đồ (Thuyết minh kỹ thuật)

1. **Kiểm tra tồn kho 2 vòng (`BR2`):** Hệ thống thực hiện kiểm tra tồn kho ngay khi mở màn hình Checkout (vòng 1 – `checkStock()`) và chốt khóa dòng lần cuối bên trong Transaction ACID trước khi trừ kho (vòng 2 – `updateStock()`), triệt tiêu hoàn toàn rủi ro bán vượt tồn kho đồng thời (overselling).
2. **Rẽ nhánh phương thức thanh toán (`BR6`):**
   - **SD2A (COD/Chuyển khoản):** Tạo đơn ngay với `trangThaiTT = 'Chưa thanh toán'` và nhân viên đối soát thực tế tại quầy/kho ở UC17.
   - **SD2B (Online):** Kích hoạt UC14 (chuyển hướng → cổng thanh toán → nhận IPN Webhook → xác thực HMAC-SHA512 → kiểm tra Idempotent).
3. **Giao dịch ACID (`FR22`):** Toàn bộ thao tác `insertOrder` → `insertOrderItems` → `updateStock` → `removeCartItems` (và `insertPayment` ở SD2B) được bọc trong một Transaction duy nhất; nếu xảy ra lỗi ở bất kỳ bước nào, hệ thống tự `ROLLBACK` – không để dữ liệu ở trạng thái nửa vời.
4. **Chốt giá tại thời điểm mua (`BR3`):** Giá trị `donGia` trong `ORDER_ITEM` được lấy từ `PRODUCT.gia` tại chính thời điểm thực thi Transaction, không bị thay đổi dù giá sản phẩm có biến động sau này.
5. **Bảo mật thanh toán online (NFR):** Mọi gói tin gửi đến `PaymentGateway` đều được ký bằng HMAC-SHA512. Khi nhận IPN Webhook, `PaymentService` kiểm tra chữ ký và cơ chế Idempotency (`ma_giao_dich` đã xử lý chưa) trước khi cho phép cập nhật đơn hàng, chống tấn công replay và duplicate webhook.

---

## Đối chiếu với Class Diagram của Quang

Bảng đối chiếu đảm bảo thống nhất 100% giữa thông điệp trong sơ đồ tuần tự và phương thức trong mô hình lớp [03_class_diagram.md](../03_class_diagram.md):

| Thông điệp trong sơ đồ | Lớp tiếp nhận (Class) | Phương thức trong Class Diagram | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- | :--- |
| `checkStock(soLuong)` | `Product` | `+checkStock(soLuong): boolean` | Kiểm tra từng sản phẩm trong giỏ còn đủ tồn kho không (`BR2`). |
| `isValid(tongTien)` | `Coupon` | `+isValid(tongTien): boolean` | Kiểm tra mã giảm giá còn hạn, đủ điều kiện đơn tối thiểu (`BR5`). |
| `apply(tongTien)` | `Coupon` | `+apply(tongTien): decimal` | Tính số tiền chiết khấu thực tế sau khi mã hợp lệ (`BR5`). |
| `calculateTotal(subTotal, phiShip, giamGia)` | `Order` | `+calculateTotal(): decimal` | Tổng hợp `tongTienHang + phiShip - giamGia` thành tổng thanh toán cuối. |
| `pay()` | `Payment` | `+pay(): boolean` | Kích hoạt giao dịch tại cổng thanh toán, trả về trạng thái thành công / thất bại (UC14). |
| `updateStatus(orderId, trangThai)` | `Order` | `+updateStatus(trangThai): void` | Cập nhật trạng thái đơn hàng sau khi tạo đơn hoặc IPN Webhook xác thực thành công. |
| `updateStock(productIds, soLuong)` | `Product` | `+updateStock(delta): void` | Trừ số lượng tồn kho trong bảng `PRODUCT` ngay khi đơn hàng được tạo thành công (`BR2`). |
| `removeCartItems(cartId)` | `Cart` | `+removeItem(sp): void` | Xóa các `CART_ITEM` đã mua khỏi giỏ hàng sau khi tạo đơn thành công. |
