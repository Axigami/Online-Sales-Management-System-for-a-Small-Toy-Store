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
    actor Staff as Nhan vien<br/>(Staff/Admin)
    participant UI as :OrderUI<br/>(Man hinh A3/A4)
    participant Ctrl as :OrderController
    participant OrdSvc as :OrderService
    participant DB as :Database

    %% BƯỚC 1: Xem danh sách đơn chờ xác nhận (màn hình A3)
    Staff->>UI: Mo "Quan ly don hang" – tab "Cho xac nhan"
    activate UI
    UI->>Ctrl: getOrderList(trangThai="Cho xac nhan")
    activate Ctrl
    Ctrl->>DB: queryOrders(trangThai="Cho xac nhan")
    activate DB
    Note over DB: SELECT * FROM ORDERS<br/>WHERE trang_thai = 'Cho xac nhan'<br/>ORDER BY ngay_tao DESC
    DB-->>Ctrl: orderList[ ] (ma don, ten KH, tong tien, PTTT)
    deactivate DB
    Ctrl-->>UI: 200 OK (orderList)
    UI-->>Staff: Hien thi man hinh A3 (danh sach don "Cho xac nhan")

    %% BƯỚC 2: Xem chi tiết một đơn hàng (màn hình A4)
    Staff->>UI: Nhan vao don hang de xem chi tiet
    UI->>Ctrl: getOrderDetail(orderId)
    Ctrl->>DB: queryOrderDetail(orderId)
    activate DB
    Note over DB: SELECT o.*, oi.*, p.ten, p.gia<br/>FROM ORDERS o JOIN ORDER_ITEM oi<br/>JOIN PRODUCT p WHERE o.id = orderId
    DB-->>Ctrl: orderDetail (san pham, dia chi, PTTT, trang_thai_tt)
    deactivate DB
    Ctrl-->>UI: 200 OK (orderDetail)
    UI-->>Staff: Hien thi man hinh A4 (chi tiet don hang)

    %% BƯỚC 3: Kiểm tra hàng thực tế → rẽ nhánh (S2 trong AD2)
    Note over Staff: Kiem tra hang thuc te tai quay (S2 trong AD2):<br/>dam bao do choi nguyen ven, du so luong

    alt Hang du & dat chuan – Xac nhan don (Luong chinh UC17)

        Staff->>UI: Nhan "Xac nhan don hang" tren A4
        UI->>Ctrl: confirmOrder(orderId, staffId)
        Ctrl->>DB: checkOrderStatus(orderId)
        activate DB
        Note over DB: Kiem tra ORDERS.trang_thai = 'Cho xac nhan'<br/>(Chong xung dot dong thoi – 6-E1 trong UC17)
        DB-->>Ctrl: currentStatus
        deactivate DB

        Ctrl->>OrdSvc: updateStatus(orderId, "Da xac nhan")
        activate OrdSvc
        Ctrl->>DB: updateOrderStatus(orderId, trangThai, staffId, thoiGian)
        activate DB
        Note over DB: UPDATE ORDERS SET trang_thai='Da xac nhan',<br/>ma_nv_duyet=staffId, thoi_gian_duyet=NOW()
        DB-->>Ctrl: updateSuccess
        deactivate DB
        OrdSvc-->>Ctrl: statusUpdated
        deactivate OrdSvc

        Ctrl-->>UI: 200 OK (trangThai="Da xac nhan")
        UI-->>Staff: Cap nhat A4 – nhan "Da xac nhan", mo nut "Bat dau dong goi"
        Note over UI: He thong gui email xac nhan den KH<br/>(SYS3 trong AD2)

    else Hang hong / Lech kho – Tu choi don (Luong 6a UC17)

        Staff->>UI: Nhan "Tu choi don" & nhap ly do
        UI->>Ctrl: rejectOrder(orderId, staffId, lyDo)
        Ctrl->>OrdSvc: cancel(orderId)
        activate OrdSvc

        Ctrl->>DB: beginTransaction()
        activate DB
        Ctrl->>DB: updateOrderCancelled(orderId, lyDo)
        Note over DB: UPDATE ORDERS SET trang_thai='Da huy',<br/>ly_do_huy=lyDo
        DB-->>Ctrl: orderCancelled

        Ctrl->>DB: restoreStock(orderItems)
        Note over DB: UPDATE PRODUCT SET ton_kho = ton_kho + so_luong<br/>cho tung san pham trong ORDER_ITEM (BR2)
        DB-->>Ctrl: stockRestored

        Ctrl->>DB: insertInventoryLog(orderItems, lyDo)
        Note over DB: INSERT INTO INVENTORY_LOG<br/>(ly_do='Huy don / Hong hang', bien_dong=+soLuong)
        DB-->>Ctrl: logCreated

        Ctrl->>DB: commitTransaction()
        deactivate DB

        OrdSvc-->>Ctrl: cancelDone
        deactivate OrdSvc

        Ctrl-->>UI: 200 OK (trangThai="Da huy")
        UI-->>Staff: Cap nhat A4 – nhan "Da huy"
        Note over UI: He thong gui email huy don & ly do den KH<br/>(SYS1-SYS2 trong AD2)

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
    actor Staff as Nhan vien<br/>(Staff/Admin)
    participant UI as :OrderUI<br/>(Man hinh A4)
    participant Ctrl as :OrderController
    participant OrdSvc as :OrderService
    participant ShipSvc as :ShipmentService
    participant Carrier as :CarrierAPI<br/>(GHN/GHTK)
    participant DB as :Database

    %% BƯỚC 1: Mở form tạo vận đơn
    Staff->>UI: Nhan "Tao van don" tren A4
    activate UI
    UI->>Ctrl: initShipmentForm(orderId)
    activate Ctrl
    Ctrl->>DB: checkPrerequisite(orderId)
    activate DB
    Note over DB: Kiem tra:<br/>1. trang_thai IN ('Da xac nhan','Dang dong goi')<br/>2. PTTT=Online thi trang_thai_tt='Da thanh toan'<br/>3. Chua co ban ghi SHIPMENT
    DB-->>Ctrl: prerequisiteOK (orderInfo, addressInfo, tienCOD)
    deactivate DB
    Ctrl-->>UI: 200 OK (shipmentFormData)
    UI-->>Staff: Hien thi form tao van don (carrier, trong luong, COD, ghi chu)

    %% BƯỚC 2: Sinh mã vận đơn & gửi sang Carrier API
    Staff->>UI: Chon carrier (GHN/GHTK) & nhan "Xac nhan tao van don"
    UI->>Ctrl: createShipment(orderId, carrierId, shipmentParams)

    Ctrl->>ShipSvc: generateTrackingCode(orderId)
    activate ShipSvc
    Note over ShipSvc: Sinh ma van don: WH-YYYYMMDD-XXXX<br/>(WH = tien to kho, YYYYMMDD = ngay tao,<br/>XXXX = so thu tu tang dan trong ngay – FR26)
    ShipSvc-->>Ctrl: maVanDon (WH-YYYYMMDD-XXXX)
    deactivate ShipSvc

    Ctrl->>Carrier: registerShipment(maVanDon, addressInfo, shipmentParams, tienCOD)
    activate Carrier
    Note over Carrier: Carrier xac thuc tuyen giao, kiem tra dia chi<br/>va tien thu ho COD (5-E1 / 5-E2 / 5-E3 trong UC21)
    Carrier-->>Ctrl: carrierResult (status, carrierShipmentId / errorCode)
    deactivate Carrier

    %% BƯỚC 3: Rẽ nhánh theo kết quả Carrier
    alt Carrier chap nhan van don (status=200)

        Ctrl->>ShipSvc: createShipment()
        activate ShipSvc
        Ctrl->>DB: insertShipment(orderId, carrierId, maVanDon, phi, tienCOD)
        activate DB
        Note over DB: INSERT INTO SHIPMENT<br/>(order_id, carrier_id, ma_van_don,<br/>phi, trang_thai='Cho lay hang', tien_cod)
        DB-->>Ctrl: shipmentId
        deactivate DB
        ShipSvc-->>Ctrl: shipmentCreated
        deactivate ShipSvc

        Ctrl-->>UI: 201 Created (shipmentId, maVanDon)
        UI-->>Staff: Hien thi phieu van don & nut "In phieu giao hang"
        Note over UI: Xuat Shipping Label (barcode + QR)<br/>de in dan len kien hang do choi

        %% BƯỚC 4: Bàn giao Shipper (C2 trong AD2)
        Note over Staff: Shipper den quan nhan kien hang (C2 trong AD2)
        Staff->>UI: Nhan "Ban giao cho Shipper" tren A4
        UI->>Ctrl: handoverToShipper(orderId, shipmentId)
        Ctrl->>OrdSvc: updateStatus(orderId, "Dang giao")
        activate OrdSvc
        Ctrl->>DB: updateHandoverStatus(orderId, shipmentId)
        activate DB
        Note over DB: UPDATE SHIPMENT SET trang_thai='Dang van chuyen'<br/>UPDATE ORDERS SET trang_thai='Dang giao'
        DB-->>Ctrl: handoverDone
        deactivate DB
        OrdSvc-->>Ctrl: statusUpdated
        deactivate OrdSvc
        Ctrl-->>UI: 200 OK (trangThai="Dang giao", maVanDon)
        UI-->>Staff: Cap nhat A4 – "Dang giao", hien ma tracking
        Note over UI: He thong gui email + ma tracking cho KH<br/>(SYS5 trong AD2)

    else Carrier tu choi (Timeout / Dia chi loi / Qua tai trong)

        Ctrl-->>UI: 502 Bad Gateway (errorCode, message)
        UI-->>Staff: Thong bao loi Carrier – "Thu lai hoac doi carrier khac"
        Note over UI: Don giu nguyen trang thai "Da xac nhan"<br/>Nhan vien co the thu lai hoac chon carrier khac<br/>(5-E1 / 5-E2 / 5-E3 trong UC21)

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
