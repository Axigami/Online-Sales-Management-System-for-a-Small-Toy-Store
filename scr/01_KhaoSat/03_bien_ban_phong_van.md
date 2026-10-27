# Biên bản phỏng vấn chủ shop

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

## Buổi 1

| Mục | Nội dung |
| --- | --- |
| Ngày giờ | [DD/MM/YYYY HH:mm] |
| Người được phỏng vấn | [Tên chủ shop / Người quản lý] |
| Cửa hàng | [Tên cửa hàng đồ chơi] |
| Người phỏng vấn | [Tên thành viên nhóm] |
| Hình thức | Trực tiếp / Gọi video / Điện thoại |

| # | Câu hỏi | Trả lời tóm tắt |
| --- | --- | --- |
| 1 | **Nhận đơn qua kênh nào, mỗi ngày bao nhiêu đơn?** | [Ví dụ: Facebook Messenger, Zalo, điện thoại. Trung bình 15-20 đơn/ngày, cuối tuần lên 30-40 đơn] |
| 2 | **Quản lý tồn kho thế nào, hay sai ở đâu?** | [Ví dụ: Theo dõi trên Excel, cập nhật cuối ngày. Hay sai do: quên cập nhật khi bán qua Zalo, nhập kho không kịp ghi, không có phiếu xuất chuẩn] |
| 3 | **Giao hàng/thu tiền COD gặp khó gì?** | [Ví dụ: Shipper lấy hàng chậm, khách từ chối nhận/hủy đơn lúc shipper đã đến, shipper thu tiền chậm chuyển về, địa chỉ khách sai/không liên lạc được] |
| 4 | **Cần xem báo cáo gì?** | [Ví dụ: Doanh thu ngày/tuần/tháng, top 10 SP bán chạy, tồn kho thấp, đơn hàng theo trạng thái, doanh thu theo kênh] |
| 5 | **Muốn hệ thống làm được gì đầu tiên?** | [Ví dụ: 1) Tự động tạo đơn từ web, 2) Kiểm tra tồn kho real-time, 3) Tạo vận đơn in mã QR, 4) Báo cáo doanh thu tự động, 5) Khách tự tra cứu đơn] |

## Buổi 2 (nếu có)

| Mục | Nội dung |
| --- | --- |
| Ngày giờ | [DD/MM/YYYY HH:mm] |
| Người được phỏng vấn | [Tên chủ shop / Người quản lý] |
| Cửa hàng | [Tên cửa hàng đồ chơi] |
| Người phỏng vấn | [Tên thành viên nhóm] |
| Hình thức | Trực tiếp / Gọi video / Điện thoại |

| # | Câu hỏi | Trả lời tóm tắt |
| --- | --- | --- |
| 1 | **Quy trình xử lý đơn hàng lỗi/hủy như thế nào?** | [Ví dụ: Nhận biết qua shipper báo / khách nhắn. Hoàn kho thủ công, không có log. Đôi khi quên hoàn kho.] |
| 2 | **Coupon/khuyến mãi hiện tại áp dụng như thế nào?** | [Ví dụ: Giảm giá trực tiếp trên giá bán, hoặc tặng kèm. Không có mã coupon, áp dụng tùy hứng.] |
| 3 | **Phí ship tính thế nào? Có bảng giá không?** | [Ví dụ: Tùy khu vực, shipper báo giá. Chưa có bảng cố định. Miễn phí ship đơn > 500k (chưa chắc).] |
| 4 | **Phân quyền nhân viên mong muốn?** | [Ví dụ: Nhân viên bán: xem/confirm đơn. Nhân viên kho: nhập/xuất kho. Kế toán: xem báo cáo. Admin: full quyền.] |
| 5 | **Tích hợp ngân hàng/shipper thật có cần không?** | [Ví dụ: Chuyển khoản qua Vietcombank/Techcombank. Shipper dùng GHN/GHTK. Prototype mô phỏng trước, thật sau.] |

## Tổng hợp rút ra

### Vấn đề nghiệp vụ chính
1. **Đơn hàng phân tán** qua Zalo/FB/điện thoại → cần tập trung vào 1 hệ thống.
2. **Tồn kho không real-time** → oversell, chợt hàng, khó báo cáo.
3. **Giao hàng COD rủi ro cao** → khách hủy, shipper chậm thu tiền, đối soát thủ công.
4. **Thiếu báo cáo tự động** → chủ shop mất thời gian gộp Excel, không kịp quyết định.
5. **Không có quy trình chuẩn cho ngoại lệ** → xử lý ad-hoc, quên hoàn kho, tranh chấp không bằng chứng.

### Yêu cầu ưu tiên cao (Must-have)
- Giao diện khách: duyệt SP, giỏ hàng, checkout 1 trang (địa chỉ, ship, thanh toán, coupon), tra cứu đơn.
- Giao diện nhân viên: danh sách đơn chờ xác nhận (check tồn kho, địa chỉ, coupon, phí ship), đóng gói & tạo vận đơn, xử lý ngoại lệ.
- Giao diện quản trị: CRUD SP/danh mục, quản lý tồn kho (nhập/xuất/cảnh báo), coupon & phí ship, báo cáo dashboard, phân quyền RBAC.
- Tích hợp mô phỏng: cổng thanh toán (success/fail/pending), hãng ship (cập nhật trạng thái qua webhook).

### Quy tắc nghiệp vụ cần xác nhận
- [ ] Ngưỡng miễn phí ship: 500.000đ (có đúng không?)
- [ ] Phí ship: bảng giá theo khu vực (tỉnh/huyện) + khối lượng (kg) — cần bảng giá cụ thể.
- [ ] Coupon: cho phép stack (gộp nhiều mã) hay không stack?
- [ ] Tồn kho: trừ kho tại lúc nào? (Checkout / Xác nhận đơn / Đóng gói)
- [ ] Hủy đơn: hoàn kho tự động hay nhân viên xác nhận?
- [ ] Mã vận đơn format: `WH-YYYYMMDD-XXXX` (đồng ý không?)

### Ghi chú
- Biên bản này cần chủ shop ký xác nhận (chữ ký hoặc xác nhận qua Zalo/email).
- Mỗi yêu cầu rút ra phải mapping sang mã FR/NFR/BR trong file `06_nhu_cau_yeu_cau.md`.

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

## Buổi 1

| Mục | Nội dung |
| --- | --- |
| Ngày giờ | [DD/MM/YYYY HH:mm] |
| Người được phỏng vấn | [Tên chủ shop / Người quản lý] |
| Cửa hàng | [Tên cửa hàng đồ chơi] |
| Người phỏng vấn | [Tên thành viên nhóm] |
| Hình thức | Trực tiếp / Gọi video / Điện thoại |

| # | Câu hỏi | Trả lời tóm tắt |
| --- | --- | --- |
| 1 | **Nhận đơn qua kênh nào, mỗi ngày bao nhiêu đơn?** | [Ví dụ: Facebook Messenger, Zalo, điện thoại. Trung bình 15-20 đơn/ngày, cuối tuần lên 30-40 đơn] |
| 2 | **Quản lý tồn kho thế nào, hay sai ở đâu?** | [Ví dụ: Theo dõi trên Excel, cập nhật cuối ngày. Hay sai do: quên cập nhật khi bán qua Zalo, nhập kho không kịp ghi, không có phiếu xuất chuẩn] |
| 3 | **Giao hàng/thu tiền COD gặp khó gì?** | [Ví dụ: Shipper lấy hàng chậm, khách từ chối nhận/hủy đơn lúc shipper đã đến, shipper thu tiền chậm chuyển về, địa chỉ khách sai/không liên lạc được] |
| 4 | **Cần xem báo cáo gì?** | [Ví dụ: Doanh thu ngày/tuần/tháng, top 10 SP bán chạy, tồn kho thấp, đơn hàng theo trạng thái, doanh thu theo kênh] |
| 5 | **Muốn hệ thống làm được gì đầu tiên?** | [Ví dụ: 1) Tự động tạo đơn từ web, 2) Kiểm tra tồn kho real-time, 3) Tạo vận đơn in mã QR, 4) Báo cáo doanh thu tự động, 5) Khách tự tra cứu đơn] |

## Buổi 2 (nếu có)

| Mục | Nội dung |
| --- | --- |
| Ngày giờ | [DD/MM/YYYY HH:mm] |
| Người được phỏng vấn | [Tên chủ shop / Người quản lý] |
| Cửa hàng | [Tên cửa hàng đồ chơi] |
| Người phỏng vấn | [Tên thành viên nhóm] |
| Hình thức | Trực tiếp / Gọi video / Điện thoại |

| # | Câu hỏi | Trả lời tóm tắt |
| --- | --- | --- |
| 1 | **Quy trình xử lý đơn hàng lỗi/hủy như thế nào?** | [Ví dụ: Nhận biết qua shipper báo / khách nhắn. Hoàn kho thủ công, không có log. Đôi khi quên hoàn kho.] |
| 2 | **Coupon/khuyến mãi hiện tại áp dụng như thế nào?** | [Ví dụ: Giảm giá trực tiếp trên giá bán, hoặc tặng kèm. Không có mã coupon, áp dụng tùy hứng.] |
| 3 | **Phí ship tính thế nào? Có bảng giá không?** | [Ví dụ: Tùy khu vực, shipper báo giá. Chưa có bảng cố định. Miễn phí ship đơn > 500k (chưa chắc).] |
| 4 | **Phân quyền nhân viên mong muốn?** | [Ví dụ: Nhân viên bán: xem/confirm đơn. Nhân viên kho: nhập/xuất kho. Kế toán: xem báo cáo. Admin: full quyền.] |
| 5 | **Tích hợp ngân hàng/shipper thật có cần không?** | [Ví dụ: Chuyển khoản qua Vietcombank/Techcombank. Shipper dùng GHN/GHTK. Prototype mô phỏng trước, thật sau.] |

## Tổng hợp rút ra

### Vấn đề nghiệp vụ chính
1. **Đơn hàng phân tán** qua Zalo/FB/điện thoại → cần tập trung vào 1 hệ thống.
2. **Tồn kho không real-time** → oversell, chợt hàng, khó báo cáo.
3. **Giao hàng COD rủi ro cao** → khách hủy, shipper chậm thu tiền, đối soát thủ công.
4. **Thiếu báo cáo tự động** → chủ shop mất thời gian gộp Excel, không kịp quyết định.
5. **Không có quy trình chuẩn cho ngoại lệ** → xử lý ad-hoc, quên hoàn kho, tranh chấp không bằng chứng.

### Yêu cầu ưu tiên cao (Must-have)
- Giao diện khách: duyệt SP, giỏ hàng, checkout 1 trang (địa chỉ, ship, thanh toán, coupon), tra cứu đơn.
- Giao diện nhân viên: danh sách đơn chờ xác nhận (check tồn kho, địa chỉ, coupon, phí ship), đóng gói & tạo vận đơn, xử lý ngoại lệ.
- Giao diện quản trị: CRUD SP/danh mục, quản lý tồn kho (nhập/xuất/cảnh báo), coupon & phí ship, báo cáo dashboard, phân quyền RBAC.
- Tích hợp mô phỏng: cổng thanh toán (success/fail/pending), hãng ship (cập nhật trạng thái qua webhook).

### Quy tắc nghiệp vụ cần xác nhận
- [ ] Ngưỡng miễn phí ship: 500.000đ (có đúng không?)
- [ ] Phí ship: bảng giá theo khu vực (tỉnh/huyện) + khối lượng (kg) — cần bảng giá cụ thể.
- [ ] Coupon: cho phép stack (gộp nhiều mã) hay không stack?
- [ ] Tồn kho: trừ kho tại lúc nào? (Checkout / Xác nhận đơn / Đóng gói)
- [ ] Hủy đơn: hoàn kho tự động hay nhân viên xác nhận?
- [ ] Mã vận đơn format: `WH-YYYYMMDD-XXXX` (đồng ý không?)

### Ghi chú
- Biên bản này cần chủ shop ký xác nhận (chữ ký hoặc xác nhận qua Zalo/email).
- Mỗi yêu cầu rút ra phải mapping sang mã FR/NFR/BR trong file `06_nhu_cau_yeu_cau.md`. 
