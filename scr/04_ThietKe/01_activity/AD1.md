# AD1 – Đặt hàng và thanh toán

> **Phụ trách:** Tô Đoàn Khánh Nguyên  
> **Trạng thái:** 🟠 Chờ duyệt  
> **Hạn:** 2026-11-05  

Tài liệu mô hình hóa luồng công việc của quy trình **Đặt hàng và thanh toán (Checkout & Payment Flow)** thông qua sơ đồ hoạt động phân làn (Activity Diagram with Swimlanes). Sơ đồ thể hiện sự phối hợp nhịp nhàng giữa thao tác người dùng, xử lý logic nội bộ và tích hợp cổng thanh toán trực tuyến.

* **Ca sử dụng liên quan:** `UC13 (Đặt hàng)`, `UC14 (Thanh toán online)`, `UC12 (Áp mã giảm giá)`, `UC20 (Tính phí vận chuyển)`, `UC02 (Đăng nhập)`.
* **Phân chia 3 làn bơi (Swimlanes):**
  1. `Khách hàng (Customer)`: Tác nhân khởi tạo và xác nhận giao dịch.
  2. `Hệ thống (System)`: Xử lý kiểm tra tồn kho, tính cước phí, áp voucher, quản lý giao dịch CSDL.
  3. `Cổng thanh toán (Payment Gateway)`: Hệ thống ngoài chịu trách nhiệm xử lý trừ tiền qua thẻ/ví điện tử.

---

## 1. Sơ đồ hoạt động AD1 (Activity Diagram)

```mermaid
flowchart TB
    %% =========================
    %% ĐỊNH NGHĨA CÁC SWIMLANES
    %% =========================
    
    subgraph Lane_Customer ["Khách hàng (Customer)"]
        direction TB
        A1(["Bắt đầu: Bấm 'Đặt hàng'"]) --> A2["Chọn hoặc nhập địa chỉ giao hàng"]
        A3["Chọn phương thức thanh toán\n& nhập mã giảm giá (nếu có)"]
        A4["Kiểm tra lại hóa đơn và\nbấm 'Xác nhận thanh toán'"]
        A5["Thực hiện xác thực thanh toán\n(OTP / Quét mã QR)"]
        A6["Nhận thông báo đặt hàng thành công\n& mã đơn hàng"]
        A7["Nhận thông báo lỗi thanh toán\n& chọn thanh toán lại"]
        A8["Xem thông báo hết hàng\nvà chỉnh sửa lại giỏ hàng"]
    end

    subgraph Lane_System ["Hệ thống (System)"]
        direction TB
        B1{"Kiểm tra tồn kho\n(Real-time)?"}
        B2["Tính toán phí vận chuyển (UC20)\ntheo địa chỉ và trọng lượng"]
        B3["Validate coupon (UC12)\nvà tính lại tổng tiền thanh toán"]
        B4{"Phương thức\nthanh toán?"}
        B5["Khởi tạo giao dịch thanh toán\nvà chuyển hướng sang cổng"]
        B6{"Xác thực kết quả\ntừ cổng thanh toán?"}
        B7["Tạo đơn hàng mới (Trạng thái: Chờ xác nhận)\nvà trừ tồn kho (Transaction ACID)"]
        B8["Xóa các món đồ chơi đã mua khỏi giỏ hàng"]
    end

    subgraph Lane_Gateway ["Cổng thanh toán (Payment Gateway)"]
        direction TB
        C1["Hiển thị giao diện cổng thanh toán\n(Mô phỏng VNPAY/MoMo)"]
        C2{"Xử lý giao dịch\nthành công?"}
        C3["Gửi tín hiệu IPN / Callback (Success)\nvề hệ thống"]
        C4["Gửi tín hiệu IPN / Callback (Failed)\nvề hệ thống"]
    end

    %% =========================
    %% LUỒNG KẾT NỐI GIỮA CÁC LÀN
    %% =========================

    A1 --> B1
    
    %% Rẽ nhánh tồn kho
    B1 -- "Không đủ hàng" --> A8
    A8 --> End_Fail(["Kết thúc: Sửa giỏ hàng"])
    
    B1 -- "Đủ hàng" --> A2
    A2 --> B2
    B2 --> A3
    A3 --> B3
    B3 --> A4
    A4 --> B4

    %% Rẽ nhánh phương thức thanh toán
    B4 -- "Thanh toán Online" --> B5
    B5 --> C1
    C1 --> A5
    A5 --> C2

    C2 -- "Thất bại / Hủy" --> C4
    C4 --> B6
    B6 -- "Thất bại" --> A7
    A7 --> A3

    C2 -- "Thành công" --> C3
    C3 --> B6
    B6 -- "Thành công" --> B7

    B4 -- "COD / Chuyển khoản" --> B7

    %% Hoàn tất đơn hàng
    B7 --> B8
    B8 --> A6
    A6 --> End_Success(["Kết thúc: Đặt hàng thành công"])
```

---

## 2. Giải thích sơ đồ hoạt động (Thuyết minh kỹ thuật)

1. **Kiểm tra tồn kho 2 lần (Two-phase checking):** Để triệt tiêu hoàn toàn rủi ro bán vượt tồn kho (overselling), hệ thống kích hoạt kiểm tra tồn kho ngay khi khách bấm đặt hàng và khóa dòng kiểm tra lần cuối ngay trước khi thực thi lệnh trừ kho (`B7`).
2. **Xử lý giao dịch toàn vẹn (ACID):** Bước tạo đơn hàng mới, tạo chi tiết đơn hàng (`ORDER_ITEM`) và trừ số lượng trong bảng `PRODUCT` được đóng gói trong một Database Transaction duy nhất; nếu có bất kỳ sự cố mạng nào xảy ra thì toàn bộ dữ liệu tự động rollback về trạng thái ban đầu.
3. **Rẽ nhánh thanh toán linh hoạt:** Hệ thống phân tách rõ rệt giữa luồng thanh toán trả sau (COD/Chuyển khoản) và trả trước trực tuyến. Nếu thanh toán qua Cổng gặp trục trặc (khách hủy giao dịch, thẻ không đủ số dư), hệ thống không tạo đơn lỗi mà điều hướng khách quay lại bước chọn phương thức khác để tối ưu tỷ lệ chuyển đổi đơn hàng.
