# SD2 – Đặt hàng và thanh toán online (UC13 + UC14)

> **Phụ trách:** Huỳnh Nguyễn Quốc Việt  
> **Trạng thái:** 🟠 Chờ duyệt  
> **Hạn:** —

---

## 1. Sơ đồ tuần tự (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor Khach as Khách hàng
    participant UI as :CheckoutUI<br/>(Màn hình C5)
    participant Ctrl as :OrderController
    participant OrderSvc as :OrderService
    participant Order as :Order
    participant PaySvc as :PaymentService
    participant PayGate as :PaymentGateway<br/>(VNPAY/MoMo)
    participant DB as :Database

    Khach->>UI: Chọn địa chỉ, PTTT & Bấm "Đặt hàng"
    activate UI
    UI->>Ctrl: checkout(orderData, paymentMethod)
    activate Ctrl

    %% 1. Kiểm tra tồn kho khả dụng lần cuối
    Ctrl->>OrderSvc: processOrder(orderData, paymentMethod)
    activate OrderSvc
    OrderSvc->>DB: SELECT id, ton_kho FROM PRODUCT WHERE id IN (...)
    activate DB
    DB-->>OrderSvc: listProductsStock
    deactivate DB

    alt Tồn kho không đáp ứng đủ (tonKho < requestedQty)
        OrderSvc-->>Ctrl: stockError ("Một số sản phẩm không đủ tồn kho")
        Ctrl-->>UI: 400 Bad Request
        UI-->>Khach: Cảnh báo hết hàng & yêu cầu chỉnh sửa giỏ (AD1: B1 -> A8)
    else Đủ tồn kho khả dụng
        %% 2. Tính tổng tiền đơn hàng
        OrderSvc->>Order: calculateTotal(items, shippingFee, discount)
        activate Order
        Order-->>OrderSvc: totalAmount
        deactivate Order

        %% 3. Tạo đơn hàng và trừ tồn kho (ACID Transaction)
        OrderSvc->>DB: BEGIN TRANSACTION
        activate DB
        OrderSvc->>DB: INSERT INTO ORDERS (trang_thai='Chờ xác nhận', tong_tien, ...)
        OrderSvc->>DB: INSERT INTO ORDER_ITEM (order_id, product_id, so_luong, don_gia)
        OrderSvc->>DB: UPDATE PRODUCT SET ton_kho = ton_kho - so_luong (Trừ tồn BR2)
        OrderSvc->>DB: DELETE FROM CART_ITEM WHERE cart_id = userCartId (Xóa giỏ)
        OrderSvc->>DB: COMMIT TRANSACTION
        DB-->>OrderSvc: transactionSuccess (orderId, orderCode)
        deactivate DB

        %% 4. Rẽ nhánh phương thức thanh toán
        alt Phương thức Online (Cổng thanh toán VNPAY/MoMo) - UC14
            OrderSvc->>PaySvc: createPaymentUrl(orderId, totalAmount)
            activate PaySvc
            PaySvc->>PayGate: initTransaction(orderId, totalAmount, checksum)
            activate PayGate
            PayGate-->>PaySvc: paymentUrl (kèm token xác thực)
            deactivate PayGate
            PaySvc-->>OrderSvc: paymentUrl
            deactivate PaySvc

            OrderSvc-->>Ctrl: redirectUrl (paymentUrl)
            Ctrl-->>UI: 302 Redirect to paymentUrl
            UI-->>Khach: Chuyển hướng sang giao diện Cổng thanh toán (AD1: B5 -> C1)

            %% Khách thanh toán trên cổng
            Khach->>PayGate: Xác thực thanh toán (Quét QR / OTP)
            activate PayGate

            alt Thanh toán thành công (AD1: C2 -> C3)
                PayGate->>PaySvc: webhookIPN(orderId, status='SUCCESS', signature)
                activate PaySvc
                Note over PaySvc: Xác thực chữ ký HMAC-SHA512
                PaySvc->>Order: updateStatus(orderId, 'Đã thanh toán')
                activate Order
                Order-->>PaySvc: statusUpdated
                deactivate Order
                PaySvc->>DB: INSERT INTO PAYMENT (order_id, so_tien, trang_thai='Đã thanh toán')
                activate DB
                PaySvc->>DB: UPDATE ORDERS SET trang_thai_thanh_toan = 'Đã thanh toán'
                DB-->>PaySvc: updateSuccess
                deactivate DB
                PaySvc-->>PayGate: 200 OK (IPN Ack)
                deactivate PaySvc

                PayGate-->>UI: redirectSuccess(returnUrl)
                deactivate PayGate
                UI-->>Khach: Hiển thị Màn hình C6 "Đặt hàng & Thanh toán thành công" (AD1: A6)

            else Thanh toán thất bại hoặc Khách hủy (AD1: C2 -> C4)
                activate PayGate
                PayGate->>PaySvc: webhookIPN(orderId, status='FAILED')
                activate PaySvc
                PaySvc->>DB: INSERT INTO PAYMENT (order_id, trang_thai='Thất bại')
                activate DB
                DB-->>PaySvc: logRecorded
                deactivate DB
                PaySvc-->>PayGate: 200 OK (IPN Ack)
                deactivate PaySvc

                PayGate-->>UI: redirectFailed(returnUrl)
                deactivate PayGate
                UI-->>Khach: Báo lỗi thanh toán & Cho phép đổi PTTT / Thử lại (AD1: A7)
            end

        else Phương thức COD / Chuyển khoản ngân hàng - UC13
            OrderSvc-->>Ctrl: orderSuccess (orderId, orderCode)
            Ctrl-->>UI: 200 OK (orderSummary)
            UI-->>Khach: Hiển thị Màn hình C6 "Đặt hàng thành công", mã đơn & email (AD1: A6)
        end
        deactivate OrderSvc
        deactivate Ctrl
        deactivate UI
    end
```

---

## 2. Giải thích sơ đồ (Thuyết minh kỹ thuật)

1. **Mục đích:** Sơ đồ mô hình hóa toàn diện quá trình chốt đơn và xử lý thanh toán, tích hợp liền mạch giữa hai ca sử dụng nòng cốt là `UC13 (Đặt hàng / Checkout)` và `UC14 (Thanh toán online)`.
2. **Khớp 100% với Sơ đồ hoạt động AD1 của Nguyên:**
   - Kiểm tra tồn kho thời gian thực trước khi trừ (`B1` trong `AD1`). Nếu không đủ hàng, chuyển hướng khách sửa giỏ (`A8`).
   - Tạo đơn hàng và trừ kho trong một giao dịch nguyên tử (`B7` trong `AD1` – ACID Transaction).
   - Phân nhánh phương thức thanh toán: Nếu chọn Online Payment (`B4`), hệ thống sinh URL kèm chữ ký số và chuyển tiếp khách sang Cổng thanh toán (`B5 -> C1`).
   - Nhánh kết quả thanh toán: Xử lý thành công (`C3 -> B6 -> A6`) cập nhật trạng thái `"Đã thanh toán"`; xử lý thất bại (`C4 -> A7`) cho phép thử lại hoặc đổi sang COD.
3. **Đảm bảo tính toàn vẹn dữ liệu (`BR2`, `BR3`):** 
   - Số lượng đồ chơi chỉ được trừ trong bảng `PRODUCT` khi toàn bộ đơn hàng và các dòng `ORDER_ITEM` được ghi nhận thành công.
   - Các sản phẩm đã chốt đơn được tự động dọn sạch khỏi bảng `CART_ITEM` (`B8` trong `AD1`).
4. **Bảo mật giao dịch thanh toán:** Tín hiệu Webhook IPN từ cổng thanh toán được xác thực chữ ký bảo mật HMAC-SHA512 trước khi cập nhật CSDL, ngăn chặn giả mạo kết quả thanh toán.

---

## 3. Đối chiếu với Class Diagram của Quang

Bảng đối chiếu đảm bảo sự thống nhất 100% giữa thông điệp trong sơ đồ tuần tự và phương thức trong mô hình lớp [03_class_diagram.md](../03_class_diagram.md):

| Thông điệp trong sơ đồ | Lớp tiếp nhận (Class) | Phương thức trong Class Diagram | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- | :--- |
| `calculateTotal(items, shippingFee, discount)` | `Order` | `+calculateTotal(): decimal` | Tính tổng tiền phải thanh toán = Tiền hàng + Phí ship – Giảm giá (`FR17`). |
| `updateStatus(orderId, status)` | `Order` | `+updateStatus()` | Cập nhật trạng thái đơn hàng (`ORDERS.trang_thai`) và thanh toán (`trang_thai_thanh_toan`). |
| `initTransaction(...)` / `webhookIPN(...)` | `Payment` | `+pay(): boolean` | Khởi tạo và ghi nhận kết quả xử lý thanh toán trực tuyến (`UC14`). |
| `checkAvailability(...)` | `Product` | `+checkStock(): boolean` | Quét kiểm tra tồn kho lần cuối cho toàn bộ danh sách sản phẩm trước khi tạo đơn (`BR2`). |
| `DELETE FROM CART_ITEM` | `Cart` | `+removeItem()` | Dọn sạch các mặt hàng đã mua thành công khỏi giỏ hàng cá nhân (`UC11`). |
