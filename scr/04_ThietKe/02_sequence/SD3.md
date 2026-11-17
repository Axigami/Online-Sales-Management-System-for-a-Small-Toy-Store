# SD3 – Xác nhận đơn và tạo vận đơn (UC17 + UC21)

> **Phụ trách:** Huỳnh Nguyễn Quốc Việt
> **Trạng thái:** 🟠 Chờ duyệt
> **Hạn:** —

> [!NOTE]
> File SD3 được chia thành **2 sơ đồ tuần tự con (SD3a & SD3b)** để dễ trình bày, nhưng vẫn nằm trong cùng 1 file theo đúng quy ước `SD1–SD4` của nhóm. Hai sơ đồ nối tiếp nhau qua trạng thái trung gian `'Đã xác nhận'` sau khi nhân viên hoàn tất đóng gói (`UC18`).

---

## Sơ đồ 3A – Xem & Xác nhận đơn hàng (UC17)

**Phạm vi:** Nhân viên tra cứu danh sách đơn → kiểm tra hàng thực tế → xác nhận duyệt hoặc từ chối đơn kèm hoàn tồn kho ACID.

```mermaid
sequenceDiagram
    autonumber
    actor Staff as Nhân viên<br/>(Staff/Admin)
    participant UI as :OrderUI<br/>(Màn hình A3/A4)
    participant Ctrl as :OrderController
    participant OrdSvc as :OrderService
    participant DB as :Database

    %% BƯỚC 1: Xem danh sách đơn chờ xác nhận (màn hình A3)
    Staff->>UI: Mở "Quản lý đơn hàng" – tab "Chờ xác nhận"
    activate UI
    UI->>Ctrl: getOrderList(trangThai="Chờ xác nhận")
    activate Ctrl
    Ctrl->>DB: queryOrders(trangThai="Chờ xác nhận")
    activate DB
    Note over DB: SELECT * FROM ORDERS<br/>WHERE trang_thai = 'Chờ xác nhận'<br/>ORDER BY ngay_tao DESC
    DB-->>Ctrl: orderList[ ] (mã đơn, tên KH, tổng tiền, PTTT)
    deactivate DB
    Ctrl-->>UI: 200 OK (orderList)
    UI-->>Staff: Hiển thị màn hình A3 (danh sách đơn "Chờ xác nhận")

    %% BƯỚC 2: Xem chi tiết một đơn hàng (màn hình A4)
    Staff->>UI: Nhấn vào đơn hàng để xem chi tiết
    UI->>Ctrl: getOrderDetail(orderId)
    Ctrl->>DB: queryOrderDetail(orderId)
    activate DB
    Note over DB: SELECT o.*, oi.*, p.ten, p.gia<br/>FROM ORDERS o JOIN ORDER_ITEM oi<br/>JOIN PRODUCT p WHERE o.id = orderId
    DB-->>Ctrl: orderDetail (sản phẩm, địa chỉ, PTTT, trang_thai_tt)
    deactivate DB
    Ctrl-->>UI: 200 OK (orderDetail)
    UI-->>Staff: Hiển thị màn hình A4 (chi tiết đơn hàng)

    %% BƯỚC 3: Kiểm tra hàng thực tế → rẽ nhánh (S2 trong AD2)
    Note over Staff: Kiểm tra hàng thực tế tại quầy (S2 trong AD2):<br/>đảm bảo đồ chơi nguyên vẹn, đủ số lượng

    alt Hàng đủ & đạt chuẩn – Xác nhận đơn (Luồng chính UC17)

        Staff->>UI: Nhấn "Xác nhận đơn hàng" trên A4
        UI->>Ctrl: confirmOrder(orderId, staffId)
        Ctrl->>DB: checkOrderStatus(orderId)
        activate DB
        Note over DB: Kiểm tra ORDERS.trang_thai = 'Chờ xác nhận'<br/>(Chống xung đột đồng thời – 6-E1 trong UC17)
        DB-->>Ctrl: currentStatus
        deactivate DB

        Ctrl->>OrdSvc: updateStatus(orderId, "Đã xác nhận")
        activate OrdSvc
        Ctrl->>DB: updateOrderStatus(orderId, trangThai, staffId, thoiGian)
        activate DB
        Note over DB: UPDATE ORDERS SET trang_thai='Đã xác nhận',<br/>ma_nv_duyet=staffId, thoi_gian_duyet=NOW()
        DB-->>Ctrl: updateSuccess
        deactivate DB
        OrdSvc-->>Ctrl: statusUpdated
        deactivate OrdSvc

        Ctrl-->>UI: 200 OK (trangThai="Đã xác nhận")
        UI-->>Staff: Cập nhật A4 – nhãn "Đã xác nhận", mở nút "Bắt đầu đóng gói"
        Note over UI: Hệ thống gửi email xác nhận đến KH<br/>(SYS3 trong AD2)

    else Hàng hỏng / Lệch kho – Từ chối đơn (Luồng 6a UC17)

        Staff->>UI: Nhấn "Từ chối đơn" & nhập lý do
        UI->>Ctrl: rejectOrder(orderId, staffId, lyDo)
        Ctrl->>OrdSvc: cancel(orderId)
        activate OrdSvc

        Ctrl->>DB: beginTransaction()
        activate DB
        Ctrl->>DB: updateOrderCancelled(orderId, lyDo)
        Note over DB: UPDATE ORDERS SET trang_thai='Đã hủy',<br/>ly_do_huy=lyDo
        DB-->>Ctrl: orderCancelled

        Ctrl->>DB: restoreStock(orderItems)
        Note over DB: UPDATE PRODUCT SET ton_kho = ton_kho + so_luong<br/>cho từng sản phẩm trong ORDER_ITEM (BR2)
        DB-->>Ctrl: stockRestored

        Ctrl->>DB: insertInventoryLog(orderItems, lyDo)
        Note over DB: INSERT INTO INVENTORY_LOG<br/>(ly_do='Hủy đơn / Hỏng hàng', bien_dong=+soLuong)
        DB-->>Ctrl: logCreated

        Ctrl->>DB: commitTransaction()
        deactivate DB

        OrdSvc-->>Ctrl: cancelDone
        deactivate OrdSvc

        Ctrl-->>UI: 200 OK (trangThai="Đã hủy")
        UI-->>Staff: Cập nhật A4 – nhãn "Đã hủy"
        Note over UI: Hệ thống gửi email hủy đơn & lý do đến KH<br/>(SYS1-SYS2 trong AD2)

    end
    deactivate Ctrl
    deactivate UI
```

**→ Tiếp theo:** Nhân viên nhặt hàng & đóng gói (`UC18`), sau đó kích hoạt **SD3B** để tạo vận đơn.

---

## Giải thích SD3A (Thuyết minh kỹ thuật)

1. **Tải đơn hàng 2 bước:** Hệ thống hiển thị danh sách tóm tắt trên A3 trước, chỉ truy vấn chi tiết (`ORDER_ITEM`, `PRODUCT`, địa chỉ) khi nhân viên mở một đơn cụ thể trên A4 – giảm tải DB và tăng tốc màn hình danh sách.
2. **Chống xung đột đồng thời (Optimistic Locking):** Trước khi cập nhật, hệ thống đọc lại `ORDERS.trang_thai` từ DB (không dùng cache) để phát hiện trường hợp 2 nhân viên cùng duyệt 1 đơn – khớp ngoại lệ 6-E1 trong UC17.
3. **ACID Transaction khi hủy đơn (`BR2`):** Toàn bộ `updateOrder(hủy)` → `updateStock(+delta)` → `insertInventoryLog()` được bọc trong 1 Transaction duy nhất; nếu xảy ra lỗi ở bất kỳ bước nào thì `ROLLBACK` hoàn toàn, không để tồn kho ở trạng thái nửa vời.

---

## Đối chiếu SD3A với Class Diagram của Quang

| Thông điệp trong sơ đồ | Lớp tiếp nhận (Class) | Phương thức trong Class Diagram | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- | :--- |
| `updateStatus(orderId, "Da xac nhan")` | `Order` | `+updateStatus(trangThai): void` | Chuyển đơn sang `'Đã xác nhận'` sau khi nhân viên duyệt. |
| `cancel(orderId)` | `Order` | `+cancel(): void` | Hủy đơn và kích hoạt hoàn tồn kho (`BR2`). |
| `updateStock(delta)` via `restoreStock()` | `Product` | `+updateStock(delta): void` | Hoàn trả số lượng tồn kho (`delta = +soLuong`) khi đơn bị từ chối. |

---
---

## Sơ đồ 3B – Tạo vận đơn & Bàn giao Shipper (UC21)

**Phạm vi:** Nhân viên tạo vận đơn sau khi đóng gói → hệ thống sinh mã `WH-YYYYMMDD-XXXX` → gửi Carrier API → bàn giao Shipper, đơn chuyển sang `'Đang giao'`.

> [!IMPORTANT]
> **Tiền điều kiện bắt buộc:** Đơn hàng phải ở trạng thái `'Đã xác nhận'` hoặc `'Đang đóng gói'`. Nếu đơn thanh toán online thì `trang_thai_tt = 'Đã thanh toán'` (chống tạo vận đơn khi chưa thu tiền).

```mermaid
sequenceDiagram
    autonumber
    actor Staff as Nhân viên<br/>(Staff/Admin)
    participant UI as :OrderUI<br/>(Màn hình A4)
    participant Ctrl as :OrderController
    participant OrdSvc as :OrderService
    participant ShipSvc as :ShipmentService
    participant Carrier as :CarrierAPI<br/>(GHN/GHTK)
    participant DB as :Database

    %% BƯỚC 1: Mở form tạo vận đơn
    Staff->>UI: Nhấn "Tạo vận đơn" trên A4
    activate UI
    UI->>Ctrl: initShipmentForm(orderId)
    activate Ctrl
    Ctrl->>DB: checkPrerequisite(orderId)
    activate DB
    Note over DB: Kiểm tra:<br/>1. trang_thai IN ('Đã xác nhận','Đang đóng gói')<br/>2. PTTT=Online thì trang_thai_tt='Đã thanh toán'<br/>3. Chưa có bản ghi SHIPMENT
    DB-->>Ctrl: prerequisiteOK (orderInfo, addressInfo, tienCOD)
    deactivate DB
    Ctrl-->>UI: 200 OK (shipmentFormData)
    UI-->>Staff: Hiển thị form tạo vận đơn (carrier, trọng lượng, COD, ghi chú)

    %% BƯỚC 2: Sinh mã vận đơn & gửi sang Carrier API
    Staff->>UI: Chọn carrier (GHN/GHTK) & nhấn "Xác nhận tạo vận đơn"
    UI->>Ctrl: createShipment(orderId, carrierId, shipmentParams)

    Ctrl->>ShipSvc: generateTrackingCode(orderId)
    activate ShipSvc
    Note over ShipSvc: Sinh mã vận đơn: WH-YYYYMMDD-XXXX<br/>(WH = tiền tố kho, YYYYMMDD = ngày tạo,<br/>XXXX = số thứ tự tăng dần trong ngày – FR26)
    ShipSvc-->>Ctrl: maVanDon (WH-YYYYMMDD-XXXX)
    deactivate ShipSvc

    Ctrl->>Carrier: registerShipment(maVanDon, addressInfo, shipmentParams, tienCOD)
    activate Carrier
    Note over Carrier: Carrier xác thực tuyến giao, kiểm tra địa chỉ<br/>và tiền thu hộ COD (5-E1 / 5-E2 / 5-E3 trong UC21)
    Carrier-->>Ctrl: carrierResult (status, carrierShipmentId / errorCode)
    deactivate Carrier

    %% BƯỚC 3: Rẽ nhánh theo kết quả Carrier
    alt Carrier chấp nhận vận đơn (status=200)

        Ctrl->>ShipSvc: createShipment()
        activate ShipSvc
        Ctrl->>DB: insertShipment(orderId, carrierId, maVanDon, phi, tienCOD)
        activate DB
        Note over DB: INSERT INTO SHIPMENT<br/>(order_id, carrier_id, ma_van_don,<br/>phi, trang_thai='Chờ lấy hàng', tien_cod)
        DB-->>Ctrl: shipmentId
        deactivate DB
        ShipSvc-->>Ctrl: shipmentCreated
        deactivate ShipSvc

        Ctrl-->>UI: 201 Created (shipmentId, maVanDon)
        UI-->>Staff: Hiển thị phiếu vận đơn & nút "In phiếu giao hàng"
        Note over UI: Xuất Shipping Label (barcode + QR)<br/>để in dán lên kiện hàng đồ chơi

        %% BƯỚC 4: Bàn giao Shipper (C2 trong AD2)
        Note over Staff: Shipper đến quán nhận kiện hàng (C2 trong AD2)
        Staff->>UI: Nhấn "Bàn giao cho Shipper" trên A4
        UI->>Ctrl: handoverToShipper(orderId, shipmentId)
        Ctrl->>OrdSvc: updateStatus(orderId, "Đang giao")
        activate OrdSvc
        Ctrl->>DB: updateHandoverStatus(orderId, shipmentId)
        activate DB
        Note over DB: UPDATE SHIPMENT SET trang_thai='Đang vận chuyển'<br/>UPDATE ORDERS SET trang_thai='Đang giao'
        DB-->>Ctrl: handoverDone
        deactivate DB
        OrdSvc-->>Ctrl: statusUpdated
        deactivate OrdSvc
        Ctrl-->>UI: 200 OK (trangThai="Đang giao", maVanDon)
        UI-->>Staff: Cập nhật A4 – "Đang giao", hiện mã tracking
        Note over UI: Hệ thống gửi email + mã tracking cho KH<br/>(SYS5 trong AD2)

    else Carrier từ chối (Timeout / Địa chỉ lỗi / Quá tải trọng)

        Ctrl-->>UI: 502 Bad Gateway (errorCode, message)
        UI-->>Staff: Thông báo lỗi Carrier – "Thử lại hoặc đổi carrier khác"
        Note over UI: Đơn giữ nguyên trạng thái "Đã xác nhận"<br/>Nhân viên có thể thử lại hoặc chọn carrier khác<br/>(5-E1 / 5-E2 / 5-E3 trong UC21)

    end
    deactivate Ctrl
    deactivate UI
```

---

## Giải thích SD3B (Thuyết minh kỹ thuật)

1. **Kiểm tra tiền điều kiện 3 lớp:** Trước khi hiển thị form tạo vận đơn, hệ thống kiểm tra đồng thời: (a) trạng thái đơn hợp lệ, (b) đơn thanh toán online đã thu tiền (`trang_thai_tt = 'Đã thanh toán'` – chống tạo vận đơn khi chưa thu tiền, `BR6`), (c) chưa tồn tại bản ghi `SHIPMENT` liên kết (chống tạo vận đơn trùng lặp).
2. **Sinh mã vận đơn tự động (`FR26`):** `ShipmentService.generateTrackingCode()` tạo mã nội bộ chuẩn hóa `WH-YYYYMMDD-XXXX` trước khi chuyển sang Carrier API – khớp bước `SYS4` trong AD2 của Nguyên.
3. **Rẽ nhánh Carrier:** Kết quả API Carrier được xử lý trước `alt` (Carrier trả về rồi mới rẽ nhánh theo `status`) đảm bảo `activate/deactivate` cân bằng. Nhánh lỗi giữ nguyên trạng thái đơn, nhân viên có thể thử lại hoặc đổi carrier mà không mất dữ liệu.
4. **Toàn vẹn tiền thu hộ COD (`BR6`):** Giá trị `tienCOD` trong vận đơn được tự động tính từ tiền điều kiện: `tienCOD = tongTien` với COD, `tienCOD = 0đ` với đơn đã thanh toán online – triệt tiêu rủi ro Shipper thu tiền trùng.

---

## Đối chiếu SD3B với Class Diagram của Quang

| Thông điệp trong sơ đồ | Lớp tiếp nhận (Class) | Phương thức trong Class Diagram | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- | :--- |
| `createShipment()` | `Shipment` | `+createShipment(): void` | Khởi tạo bản ghi vận đơn trong `SHIPMENT` với mã `WH-YYYYMMDD-XXXX` và trạng thái `'Chờ lấy hàng'`. |
| `track()` | `Shipment` | `+track(): String` | Trả về mã vận đơn và trạng thái để hiển thị cho nhân viên và gửi link tracking cho khách. |
| `updateStatus(orderId, "Dang giao")` | `Order` | `+updateStatus(trangThai): void` | Cập nhật đơn hàng sang `'Đang giao'` sau khi bàn giao Shipper thành công (SYS5 trong AD2). |
