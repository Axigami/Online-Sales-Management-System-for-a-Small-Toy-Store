# AD2 – Xử lý đơn và giao hàng

> **Phụ trách:** Tô Đoàn Khánh Nguyên  
> **Trạng thái:** 🟠 Chờ duyệt  
> **Hạn:** 2026-11-05  

Tài liệu mô hình hóa luồng nghiệp vụ **Xử lý đơn hàng nội bộ và Giao nhận với đơn vị vận chuyển (Order Fulfillment & Delivery Flow)** bằng sơ đồ hoạt động phân làn (Activity Diagram with Swimlanes). Luồng này giải quyết bài toán cốt lõi của cửa hàng đồ chơi: chống sót đơn, tự động hóa quy trình vận đơn và kiểm soát hàng hoàn chặt chẽ.

* **Ca sử dụng liên quan:** `UC17 (Xem & xác nhận đơn)`, `UC18 (Cập nhật trạng thái đơn)`, `UC21 (Tạo vận đơn, bàn giao)`, `UC22 (Theo dõi & cập nhật trạng thái giao)`.
* **Phân chia 4 làn bơi (Swimlanes):**
  1. `Nhân viên (Staff)`: Tiếp nhận đơn, kiểm tra kho thực tế, đóng gói, bàn giao cho shipper và xử lý nhận hàng hoàn.
  2. `Hệ thống (System)`: Chuyển đổi trạng thái, sinh mã vận đơn, đồng bộ dữ liệu qua API/Webhook, tự động hoàn tồn kho.
  3. `Đơn vị vận chuyển (Carrier)`: Tiếp nhận thông tin vận đơn, lấy hàng tại shop, thực hiện giao vận và cập nhật trạng thái.
  4. `Khách hàng (Customer)`: Nhận đồ chơi, kiểm tra hàng và thanh toán tiền mặt (nếu là đơn COD).

---

## 1. Sơ đồ hoạt động AD2 (Activity Diagram)

```mermaid
flowchart TB
    %% =========================
    %% ĐỊNH NGHĨA 4 SWIMLANES
    %% =========================

    subgraph Lane_Staff ["Nhân viên (Staff)"]
        direction TB
        S1["Xem danh sách đơn mới (Trạng thái: Chờ xác nhận)"]
        S2{"Kiểm tra hàng\nthực tế tại quầy?"}
        S3["Bấm 'Từ chối đơn' & ghi chú lý do"]
        S4["Bấm 'Xác nhận đơn hàng' (UC17)"]
        S5["Nhặt hàng và đóng gói đồ chơi"]
        S6["Bấm 'Tạo vận đơn' (UC21)"]
        S7["Bàn giao kiện hàng cho Shipper"]
        S8["Tiếp nhận kiện hàng hoàn về quầy\n& xác nhận nhập lại kho"]
    end

    subgraph Lane_System ["Hệ thống (System)"]
        direction TB
        SYS1["Cập nhật đơn: 'Đã hủy'\nvà hoàn trả tồn kho nếu cần"]
        SYS2["Gửi email/thông báo hủy đơn đến Khách hàng"]
        SYS3["Cập nhật trạng thái đơn: 'Đã xác nhận'"]
        SYS4["Tự động sinh mã vận đơn WH-YYYYMMDD-XXXX\nvà gửi thông tin sang Carrier (UC21)"]
        SYS5["Cập nhật trạng thái đơn: 'Đang giao'\nvà gửi mã tracking cho Khách"]
        SYS6["Tiếp nhận Webhook giao thành công từ Carrier\nCập nhật đơn: 'Đã giao' + 'Đã thanh toán'"]
        SYS7["Tiếp nhận Webhook giao thất bại từ Carrier\nCập nhật đơn: 'Giao thất bại'"]
        SYS8["Cập nhật đơn: 'Hoàn về kho'\nvà tự động cộng hoàn tồn kho (Ghi INVENTORY_LOG)"]
    end

    subgraph Lane_Carrier ["Đơn vị vận chuyển (Carrier)"]
        direction TB
        C1["Tiếp nhận yêu cầu vận đơn qua API mô phỏng"]
        C2["Shipper đến quầy cửa hàng nhận kiện hàng"]
        C3["Thực hiện giao kiện hàng đến địa chỉ người nhận"]
        C4{"Khách hàng có nhận hàng\nthành công?"}
        C5["Gửi Webhook cập nhật: 'DELIVERED'\n(Đã giao hàng thành công)"]
        C6["Gửi Webhook cập nhật: 'FAILED'\n(Giao hàng không thành công)"]
        C7["Shipper chuyển hoàn kiện hàng về lại cửa hàng đồ chơi"]
    end

    subgraph Lane_Customer ["Khách hàng (Customer)"]
        direction TB
        CUST1["Nhận thông báo đơn đã hủy"]
        CUST2["Nhận đồ chơi, kiểm tra kiện hàng\n& thanh toán COD (nếu có)"]
        CUST3["Từ chối nhận hàng\nhoặc không thể liên lạc"]
    end

    %% =========================
    %% LUỒNG KẾT NỐI GIỮA CÁC LÀN
    %% =========================

    Start(["Bắt đầu: Đơn hàng mới"]) --> S1
    S1 --> S2

    %% Nhánh không đủ hàng
    S2 -- "Hàng bị hỏng / Lệch kho" --> S3
    S3 --> SYS1
    SYS1 --> SYS2
    SYS2 --> CUST1
    CUST1 --> End_Cancel(["Kết thúc: Đơn hủy"])

    %% Nhánh đủ hàng
    S2 -- "Đầy đủ hàng" --> S4
    S4 --> SYS3
    SYS3 --> S5
    S5 --> S6
    S6 --> SYS4

    %% Tạo vận đơn & lấy hàng
    SYS4 --> C1
    C1 --> C2
    C2 --> S7
    S7 --> SYS5
    SYS5 --> C3

    %% Phát hàng
    C3 --> C4
    C4 -- "Có nhận hàng" --> CUST2
    CUST2 --> C5
    C5 --> SYS6
    SYS6 --> End_Delivered(["Kết thúc: Giao hàng thành công"])

    C4 -- "Không nhận / Bom hàng" --> CUST3
    CUST3 --> C6
    C6 --> SYS7
    SYS7 --> C7
    C7 --> S8
    S8 --> SYS8
    SYS8 --> End_Returned(["Kết thúc: Đã hoàn về kho"])
```

---

## 2. Giải thích sơ đồ hoạt động (Thuyết minh kỹ thuật)

1. **Khắc phục triệt để bài toán sót đơn và lệch kho (As-Is):** Sơ đồ AD2 minh họa rõ cơ chế kiểm soát 2 vòng: Nhân viên kiểm hàng tại chỗ trước khi bấm xác nhận (`S2`), tránh tình trạng khách đã đặt nhưng kho thực tế bị hỏng hoặc thất lạc đồ chơi.
2. **Tự động hóa kết nối với Hãng vận chuyển (Carrier Integration):** Quá trình bàn giao vận đơn được tự động hóa thông qua hệ thống tự sinh mã vận đơn chuẩn hóa (`WH-YYYYMMDD-XXXX`), chuyển tiếp dữ liệu trực tiếp sang hãng vận chuyển và gửi mã tracking cho khách hàng tra cứu theo thời gian thực mà không cần nhân viên ghi chép tay.
3. **Quy trình xử lý ngược (Reverse Logistics / Hàng hoàn):** Luồng xử lý khi giao hàng thất bại được định nghĩa chặt chẽ. Hệ thống chỉ thực hiện hoàn trả số lượng vào bảng tồn kho (`SYS8`) khi nhân viên đã trực tiếp nhận lại kiện hàng vật lý tại cửa hàng và bấm xác nhận (`S8`), ngăn ngừa gian lận hoặc sai lệch số liệu tồn kho.
