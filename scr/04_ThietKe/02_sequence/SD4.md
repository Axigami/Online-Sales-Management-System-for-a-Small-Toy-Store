# SD4 – Cập nhật trạng thái giao hàng (UC22)

> **Phụ trách:** Huỳnh Nguyễn Quốc Việt  
> **Trạng thái:** 🟢 Đã hoàn thành  
> **Hạn:** —

---

## 1. Giới thiệu sơ đồ tuần tự SD4

Sơ đồ tuần tự **SD4** mô tả quá trình **Đơn vị vận chuyển (Carrier API – GHN/GHTK)** gọi Webhook Callback ngược về hệ thống để cập nhật hành trình bưu kiện đồ chơi theo thời gian thực (Real-time Tracking), đồng bộ trạng thái đơn hàng (`ORDERS`) và vận đơn (`SHIPMENT`), tự động đối soát công nợ COD, gửi email thông báo và hiển thị tiến trình trên màn hình `C7` cho Khách hàng.

```mermaid
sequenceDiagram
    autonumber
    actor Carrier as :CarrierAPI<br/>(GHN/GHTK)
    participant Ctrl as :ShipmentWebhook<br/>Controller
    participant ShipSvc as :ShipmentService
    participant OrdSvc as :OrderService
    participant DB as :Database
    participant Noti as :NotificationService
    actor Cust as Khách hàng<br/>(Customer / C7)

    %% BƯỚC 1: Tiếp nhận Webhook Callback
    Carrier->>Ctrl: POST /api/v1/webhooks/carrier (maVanDon, status, signature, cod)
    activate Ctrl
    Note over Ctrl: Xác thực chữ ký HMAC Secret (5-E1 trong UC22)<br/>+ Idempotent request (chống replay webhook)
    Ctrl->>ShipSvc: processCarrierWebhook(maVanDon, status, cod)
    activate ShipSvc
    ShipSvc->>DB: findShipmentByTracking(maVanDon)
    activate DB
    Note over DB: SELECT * FROM SHIPMENT<br/>WHERE ma_van_don = ?
    DB-->>ShipSvc: shipmentRecord (id, order_id, trang_thai)
    deactivate DB

    %% BƯỚC 2: Rẽ nhánh ALT theo kết quả giao hàng
    alt Giao hàng thành công (status == 'DELIVERED') - SYS6 trong AD2
        ShipSvc->>DB: updateShipmentStatus(shipmentId, "Đã giao", ngayGiao)
        activate DB
        Note over DB: UPDATE SHIPMENT SET trang_thai='Đã giao',<br/>ngay_giao=NOW()
        DB-->>ShipSvc: shipmentUpdated
        deactivate DB

        ShipSvc->>OrdSvc: syncDeliveredOrder(orderId, codCollected)
        activate OrdSvc
        OrdSvc->>DB: updateOrderStatus(orderId, "Đã giao", trangThaiTT)
        activate DB
        Note over DB: UPDATE ORDERS SET trang_thai='Đã giao',<br/>trang_thai_tt='Đã thanh toán' (đơn COD)
        DB-->>OrdSvc: orderUpdated
        deactivate DB
        OrdSvc-->>ShipSvc: orderSynced
        deactivate OrdSvc

        ShipSvc->>Noti: sendDeliverySuccessNotification(userId, orderId)
        activate Noti
        Note over Noti: Gửi email thông báo đơn đã giao thành công<br/>đến khách hàng (SYS6 trong AD2)
        Noti-->>ShipSvc: notificationSent
        deactivate Noti

        ShipSvc-->>Ctrl: 200 OK (processed)
        Ctrl-->>Carrier: 200 OK (Webhook Received)

    else Giao thất bại / Khách từ chối nhận (status == 'FAILED' / 'REJECTED') - 4-E1/4-E2 UC22
        ShipSvc->>DB: updateShipmentFailed(shipmentId, "Giao thất bại", lyDo)
        activate DB
        Note over DB: UPDATE SHIPMENT SET trang_thai='Giao thất bại',<br/>ly_do_that_bai=lyDo
        DB-->>ShipSvc: shipmentFailedUpdated
        deactivate DB

        ShipSvc->>OrdSvc: syncFailedOrder(orderId, lyDo)
        activate OrdSvc
        OrdSvc->>DB: updateOrderFailed(orderId, "Giao thất bại")
        activate DB
        Note over DB: UPDATE ORDERS SET trang_thai='Giao thất bại'<br/>(Chờ bưu tá hoàn về quầy để hoàn kho BR2)
        DB-->>OrdSvc: orderFailedUpdated
        deactivate DB
        OrdSvc-->>ShipSvc: orderFailedSynced
        deactivate OrdSvc

        ShipSvc->>Noti: sendDeliveryFailedNotification(userId, orderId, lyDo)
        activate Noti
        Note over Noti: Gửi email báo giao thất bại & hướng dẫn<br/>liên hệ hỗ trợ bưu tá (SYS7 trong AD2)
        Noti-->>ShipSvc: notificationSent
        deactivate Noti

        ShipSvc-->>Ctrl: 200 OK (processed)
        Ctrl-->>Carrier: 200 OK (Webhook Received)
    end
    deactivate ShipSvc
    deactivate Ctrl

    %% BƯỚC 3: Khách hàng tra cứu Timeline trên C7
    Cust->>Ctrl: Mở "Lịch sử & theo dõi đơn hàng" (Màn hình C7)
    activate Cust
    activate Ctrl
    Ctrl->>OrdSvc: getOrderTrackingTimeline(orderId)
    activate OrdSvc
    OrdSvc->>DB: queryOrderAndShipment(orderId)
    activate DB
    Note over DB: SELECT o.trang_thai, s.ma_van_don, s.ngay_giao, s.trang_thai<br/>FROM ORDERS o JOIN SHIPMENT s ON o.id=s.order_id
    DB-->>OrdSvc: timelineData (trạng thái, lịch sử mốc thời gian)
    deactivate DB
    OrdSvc-->>Ctrl: 200 OK (timelineData)
    deactivate OrdSvc
    Ctrl-->>Cust: Cập nhật Timeline C7: "Đã giao" / "Giao thất bại"
    deactivate Ctrl
    deactivate Cust
```

---

## 2. Giải thích SD4 (Thuyết minh kỹ thuật)

1. **Cơ chế Webhook Bất đồng bộ (Asynchronous Callback):** Hệ thống không cần thực hiện polling liên tục làm quá tải API của bên thứ 3. Thay vào đó, Carrier chủ động gửi tín hiệu HTTP POST đến endpoint `/api/v1/webhooks/carrier` mỗi khi Shipper phát kiện hàng thành công hoặc phát sinh sự cố tại địa chỉ nhận.
2. **Xác thực an ninh & Idempotency (`5-E1`, `5-E2` trong UC22):** `ShipmentWebhookController` kiểm tra chữ ký `HMAC Secret Key` nhằm triệt tiêu rủi ro giả mạo trạng thái bưu kiện. Đồng thời, cơ chế kiểm tra trạng thái hiện tại giúp xử lý an toàn các gói tin retry lặp lại từ đơn vị bưu chính mà không cập nhật CSDL hay thanh toán trùng lặp.
3. **Toàn vẹn đối soát công nợ COD:** Khi đơn hàng thuộc diện COD được phát thành công, hệ thống tự động đổi `ORDERS.trang_thai_thanh_toan = 'Đã thanh toán'`, đồng bộ đối soát tiền thu hộ với bưu cục mà không cần nhân viên kế toán can thiệp thủ công.
4. **Phân định hoàn hàng an toàn (`BR2`):** Khi giao thất bại (`status=FAILED`), hệ thống chỉ cập nhật đơn sang `'Giao thất bại'`. Việc hoàn tồn kho đồ chơi (`PRODUCT.ton_kho`) chỉ được kích hoạt khi bưu tá đã mang kiện hàng vật lý về quầy và nhân viên bấm xác nhận tại màn hình `A4` (khớp hoàn toàn bước `S8` trong `AD2` của Nguyên).

---

## 3. Đối chiếu SD4 với Class Diagram của Quang

| Thông điệp trong sơ đồ | Lớp tiếp nhận (Class) | Phương thức trong Class Diagram | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- | :--- |
| `updateShipmentStatus()` / `updateShipmentFailed()` | `Shipment` | `+track(): String` | Cập nhật mốc hành trình và trạng thái vận đơn trong bảng `SHIPMENT`. |
| `updateOrderStatus("Đã giao")` | `Order` | `+updateStatus(trangThai): void` | Chuyển trạng thái đơn sang `'Đã giao'` khi nhận được tín hiệu phát thành công từ bưu tá. |
| `updateOrderFailed("Giao thất bại")` | `Order` | `+updateStatus(trangThai): void` | Ghi nhận đơn hàng giao thất bại và chờ xử lý chuyển hoàn. |
| `queryOrderAndShipment(orderId)` | `Order` | `+calculateTotal(): double` | Nạp dữ liệu đơn hàng và lịch sử vận chuyển để hiển thị Timeline chi tiết cho khách trên màn hình `C7`. |
