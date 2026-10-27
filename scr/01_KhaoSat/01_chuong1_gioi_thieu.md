# Giới thiệu đề tài

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟢 Hoàn thành  
> **Hạn:** —

---

## 1.1 Giới thiệu đề tài

**Tên đề tài:** Hệ thống quản lý bán hàng trực tuyến cho cửa hàng đồ chơi nhỏ

**Bối cảnh:** Cửa hàng đồ chơi nhỏ hiện đang vận hành quy trình bán hàng thủ công qua các kênh Facebook/Zalo, ghi nhận đơn hàng trên sổ tay/Excel, quản lý tồn kho theo cảm tính, giao hàng qua shipper đơn lẻ thu tiền COD. Quy trình này dẫn đến việc sót đơn, nhầm đơn, tồn kho lệch thực tế, khách hàng không thể tự theo dõi đơn hàng, và chủ shop thiếu dữ liệu báo cáo để ra quyết định kinh doanh.

**Giải pháp đề xuất:** Xây dựng một ứng dụng web quản lý bán hàng trực tuyến bao gồm: giao diện khách hàng (duyệt sản phẩm, giỏ hàng, đặt hàng, thanh toán COD/chuyển khoản/cổng mô phỏng, theo dõi đơn), giao diện nhân viên (xác nhận đơn, đóng gói, tạo vận đơn mô phỏng, xử lý ngoại lệ), và giao diện quản trị (quản lý sản phẩm/tồn kho, coupon/phí ship, báo cáo doanh thu, phân quyền nhân viên). Hệ thống mô phỏng tích hợp cổng thanh toán và hãng vận chuyển, không xử lý giao dịch tiền thật.

**Đối tượng sử dụng:** Khách hàng (phụ huynh mua đồ chơi), Nhân viên bán hàng/kho, Chủ cửa hàng/Quản trị, Cổng thanh toán (mô phỏng), Đơn vị vận chuyển (mô phỏng).

**Công nghệ dự kiến:** Frontend: React/Next.js; Backend: Node.js (Express/NestJS) hoặc Python (FastAPI); Database: PostgreSQL; Authentication: JWT; Deployment: Docker, CI/CD cơ bản.

---

## 1.2 Mục tiêu

### Mục tiêu tổng quát
Vận dụng quy trình phân tích và thiết kế hệ thống thông tin để giải quyết bài toán quản lý bán hàng trực tuyến của cửa hàng đồ chơi nhỏ, từ khảo sát hiện trạng đến đặc tả yêu cầu, mô hình hóa (Use Case, Activity, Sequence, Class Diagram, ERD), thiết kế giao diện/prototype, và lập ma trận truy vết.

### Mục tiêu cụ thể
1. **Khảo sát hiện trạng:** Xác định stakeholder, vấn đề nghiệp vụ, nhu cầu của khách hàng, nhân viên, chủ cửa hàng.
2. **Thu thập & đặc tả yêu cầu:** Phân loại FR (chức năng), NFR (phi chức năng), BR (quy tắc nghiệp vụ), gán mức ưu tiên (MoSCoW).
3. **Mô hình hóa hệ thống:** Vẽ Use Case Diagram, Activity Diagram, Sequence Diagram, Class Diagram, ERD; bảo đảm tên tác nhân, mã chức năng, luồng nghiệp vụ nhất quán.
4. **Thiết kế giao diện & Prototype:** Xây dựng wireframe/mockup cho luồng mua hàng (khách) và xử lý đơn (nhân viên/quản trị).
5. **Ma trận truy vết (RTM):** Liên kết Yêu cầu ↔ Use Case ↔ Sơ đồ ↔ Màn hình ↔ Dữ liệu để kiểm tra tính đầy đủ.

---

## 1.3 Phạm vi

### 1.3.1 Trong phạm vi (In-scope)
| Chức năng | Mô tả chi tiết |
|-----------|----------------|
| Quản lý tài khoản & phân quyền | Đăng ký/đăng nhập khách (email/SĐT + OTP), quản lý user nội bộ (Admin, Nhân viên bán, Nhân viên kho, Kế toán), RBAC |
| Danh mục, sản phẩm, tồn kho cơ bản | CRUD danh mục cấp 2, sản phẩm (SKU, thuộc tính: độ tuổi, thương hiệu, trọng lượng), nhập/xuất/điều chỉnh kho, cảnh báo tồn thấp |
| Tìm kiếm & lọc | Từ khóa, bộ lọc đa tiêu chí (danh mục, độ tuổi, giá, thương hiệu, an toàn), phân trang |
| Giỏ hàng | Thêm/sửa/xóa, lưu localStorage (khách chưa đăng nhập), đồng bộ khi đăng nhập, kiểm tra tồn kho real-time |
| Đặt hàng & Checkout 1 trang | Nhập địa chỉ (gợi ý tỉnh/huyện/xã), chọn phương thức giao, thanh toán (COD/Chuyển khoản/Cổng mô phỏng), áp dụng coupon |
| Coupon & Khuyến mãi | CRUD coupon (%/cố định), điều kiện đơn tối thiểu, áp dụng theo SP/danh mục, giới hạn lượt/người dùng, stack/không stack |
| Phương thức thanh toán | COD, Chuyển khoản (hiển thị info ngân hàng), Cổng thanh toán mô phỏng (trả về success/fail/pending tùy config) |
| Quản lý đơn hàng | Danh sách đơn theo trạng thái, chi tiết đơn, timeline, in phiếu đóng gói PDF |
| Tạo vận đơn mô phỏng | Chọn hãng ship (GHN/GHTK/J&T mô phỏng), sinh mã `WH-YYYYMMDD-XXXX`, tính trọng lượng, tự động chuyển trạng thái "Đang giao" |
| Theo dõi trạng thái giao | Khách xem timeline trên web, mã vận đơn link sang trang hãng ship mô phỏng; hãng ship cập nhật qua API/Webhook |
| Báo cáo cơ bản | Dashboard doanh thu/đơn/AOV/chuyển đổi theo ngày/tuần/tháng/năm; Top SP bán chạy, tồn kho thấp; Xuất Excel/PDF |

### 1.3.2 Ngoài phạm vi (Out-of-scope) & Lý do
| Chức năng | Lý do |
|-----------|-------|
| Ứng dụng di động độc lập (iOS/Android) | Tập trung web responsive trước; mobile app xem xét phase 2 |
| Chat & chăm sóc khách hàng tự động | Phức độ cao, ngoài mục tiêu môn học; có thể tích hợp widget sau |
| Vận hành đa chi nhánh / đa kho | Cửa hàng nhỏ đơn lẻ; không cần phức tạp multi-warehouse |
| Đánh giá & review sản phẩm | Không phải core bán hàng; ưu tiên sau |
| Tích hợp thật cổng thanh toán (VNPAY/MoMo/API ngân hàng) | Yêu cầu KYC, hợp đồng, bảo mật PCI-DSS; prototype chỉ mô phỏng interface |
| Tích hợp thật API hãng vận chuyển (GHN/GHTK/J&T) | Cần tài khoản doanh nghiệp, ký hợp đồng; mô phỏng đủ để demo luồng |
| Nghiệp vụ đổi trả/hoàn tiền nâng cao | Phức tạp (quy trình kiểm tra hàng, hoàn tiền, hoàn kho); xem xét mở rộng |

### Giả định ban đầu (cần chủ shop xác nhận)
- Ngưỡng miễn phí giao hàng: **500.000đ** đơn hàng.
- Phí ship tính theo: **khu vực (tỉnh/huyện)** + **khối lượng (kg)**.
- Prototype **không xử lý giao dịch tiền thật**; dữ liệu minh họa **không phải kết quả vận hành thực tế**.

---
