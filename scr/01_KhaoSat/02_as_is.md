# Khảo sát hiện trạng (As-Is)

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện (cần vẽ sơ đồ draw.io)  
> **Hạn:** —

---

## 2.1 Quy trình hiện tại (As-Is)

![Quy trình As-Is](hinh/as_is.png)

> **Lưu ý:** Sơ đồ vẽ bằng draw.io, lưu hinh/as_is.drawio + hinh/as_is.png. Quy trình cần thể hiện: nhận đơn Facebook/Zalo → ghi sổ/Excel → kiểm tra hàng thủ công → đóng gói → gọi shipper → thu tiền COD → cập nhật sổ tay.

### Giải thích sơ đồ
Quy trình bán hàng hiện tại của cửa hàng đồ chơi diễn ra hoàn toàn thủ công qua 6 bước chính:

1. **Nhận đơn:** Khách đặt hàng qua Facebook Messenger, Zalo, hoặc gọi điện trực tiếp. Nhân viên ghi lại thông tin trên giấy/zalo note.
2. **Ghi đơn vào sổ/Excel:** Nhân viên nhập thủ công thông tin khách (tên, SĐT, địa chỉ), sản phẩm, số lượng vào sổ tay hoặc file Excel chung.
3. **Kiểm tra hàng & tồn kho:** Nhân viên ra kho đếm thực tế xem có đủ hàng không. Tồn kho không cập nhật real-time, thường lệch so với sổ sách.
4. **Đóng gói:** Lấy hàng, đóng gói đơn giản, ghi địa chỉ nhận lên bao bì tay.
5. **Gọi shipper:** Liên hệ shipper quen (GHTK/GHN/J&T cá nhân) qua Zalo/điện thoại, đọc địa chỉ, nhờ lấy hàng. Chưa có mã vận đơn hệ thống.
6. **Thu tiền COD & đối soát:** Shipper giao hàng, thu tiền khách, trừ phí ship, chuyển tiền về cho shop theo chu kỳ (hàng tuần/tháng). Chủ shop đối soát thủ công với sổ/Excel.

**Vấn đề cốt lõi:** Toàn bộ quy trình phụ thuộc vào con người, không có hệ thống tập trung, dẫn đến rủi ro cao ở mỗi bước.

---

## 2.2 Vấn đề – Hậu quả – Hệ thống mới giải quyết thế nào

| Vấn đề (As-Is) | Hậu quả | Hệ thống mới (To-Be) giải quyết thế nào |
|----------------|---------|------------------------------------------|
| **Sót / nhầm đơn** do ghi thủ công qua Zalo/FB, không có danh sách tập trung | Mất đơn, sai sản phẩm/số lượng, khách phàn nàn, uy tín giảm | **FR-06, FR-12:** Đơn hàng tập trung trên hệ thống, trạng thái " Chờ xác nhận\ rõ ràng, nhân viên xử lý theo queue, không bỏ sót |
| **Tồn kho lệch** do cập nhật thủ công, không real-time, không có phiếu nhập/xuất chuẩn | Bán quá hàng (oversell) hoặc chợt hàng dù còn kho, khó lập báo cáo | **FR-07, FR-20:** Kiểm tra tồn kho real-time tại checkout & xác nhận đơn; quản lý nhập/xuất/điều chỉnh kho có lịch sử, cảnh báo tồn thấp |
| **Khó theo dõi giao hàng** – không có mã vận đơn, shipper cập nhật qua tin nhắn rời rạc | Khách hỏi mãi không biết hàng đâu, nhân viên phải gọi shipper hỏi từng đơn | **FR-10, FR-16, FR-31:** Tự động sinh mã vận đơn WH-YYYYMMDD-XXXX, timeline trạng thái trên web (Khách & Nhân viên), hãng ship cập nhật qua API/Webhook |
| **Khách không tự xem được đơn** – phải nhắn/zalo hỏi nhân viên | Tải trọng cho nhân viên, trải nghiệm khách kém, mất thời gian | **UC-09, FR-09:** Trang \Đơn hàng của tôi\ cho khách tự tra cứu 24/7, xem timeline, mã vận đơn, phí ship, coupon |
| **Không có báo cáo tự động** – chủ shop phải gộp Excel thủ công cuối tháng | Chậm chạp, sai sót, không kịp điều chỉnh kinh doanh (SP bán chậm, tồn kho cao) | **FR-23, FR-24, FR-25:** Dashboard real-time doanh thu/đơn/AOV, Top SP, tồn kho thấp; xuất Excel/PDF tự động |
| **Quản lý coupon/phí ship tùy hứng** – ghi nhớ, dễ nhầm, không có điều kiện rõ | Mất doanh thu (ship miễn phí sai quy tắc), khách phàn nàn coupon không áp dụng được | **FR-08, FR-21, FR-22, BR-04, BR-05, BR-11, BR-12:** Coupon có mã, hạn dùng, điều kiện, giới hạn; phí ship cấu hình bảng giá khu vực/khối lượng, ngưỡng miễn phí 500k |
| **Phân quyền không rõ** – ai cũng xem được Excel, sửa xóa tùy ý | Rò rỉ dữ liệu, sai sót không ai chịu trách nhiệm | **FR-26, FR-27, BR-13:** RBAC: Admin / Nhân viên bán / Nhân viên kho / Kế toán, quyền theo màn hình, audit log không xóa được |
| **Thanh toán chỉ COD tiền mặt** – rủi ro từ chối nhận hàng, shipper delayed thu tiền | Dòng tiền chậm, hàng trả về nhiều, chi phí ship gấp đôi | **FR-11:** Hỗ trợ COD + Chuyển khoản (info ngân hàng) + Cổng mô phỏng; khách chọn trước, giảm từ chối đơn |
| **Không có quy trình xử lý ngoại lệ** – thanh toán lỗi, giao thất bại, khách hủy xử lý ad-hoc | Xử lý không đồng bộ, quên hoàn kho, không có bằng chứng cho tranh chấp | **UC-13, FR-17, FR-18, BR-09, NFR-05:** Trang \Đơn cần xử lý\ phân loại lỗi, hành động chuẩn (thử lại, liên hệ khách, hủy hoàn kho), audit log đầy đủ |

---

## 2.3 Tóm tắt nhu cầu cải tiến (đầu ra cho Chương 3)

| Nhóm nhu cầu | Mô tả | Mức độ |
|--------------|-------|--------|
| **Tập trung dữ liệu đơn hàng** | Một nguồn sự thật (single source of truth) cho toàn bộ đơn từ lúc đặt đến giao thành công | Bắt buộc |
| **Tồn kho real-time & có lịch sử** | Trừ kho tại checkout/xác nhận, phiếu nhập/xuất, cảnh báo ngưỡng | Bắt buộc |
| **Theo dõi đơn tự phục vụ** | Khách xem được trạng thái, mã vận đơn, timeline 24/7 không cần hỏi nhân viên | Bắt buộc |
| **Checkout 1 trang có validate** | Kiểm tra tồn kho, địa chỉ, coupon, phí ship tự động trước khi tạo đơn | Bắt buộc |
| **Vận đơn mô phỏng & tích hợp shipper** | Sinh mã, chọn hãng, webhook cập nhật trạng thái tự động | Bắt buộc |
| **Báo cáo Dashboard tự động** | Doanh thu, đơn, AOV, Top SP, tồn kho thấp – cập nhật near real-time | Bắt buộc |
| **Phân quyền & Audit log** | RBAC 4 vai trò, log mọi thao tác quan trọng không xóa được | Bắt buộc |
| **Đa phương thức thanh toán** | COD, Chuyển khoản, Cổng mô phỏng – giảm rủi ro từ chối đơn | Nên có |
| **Quản lý coupon/phí ship cấu hình** | Rule-based, chủ shop tự cấu hình không cần dev | Nên có |
| **Xử lý ngoại lệ có quy trình** | Phân loại lỗi, hành động chuẩn, ghi log, hoàn kho tự động | Nên có |
