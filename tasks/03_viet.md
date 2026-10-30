# Huỳnh Nguyễn Quốc Việt – 52300267 (Phân tích hành vi)

**Phụ trách:** Đặc tả chi tiết 8 Use Case • 4 Sequence Diagram

> Tick `[x]` khi xong từng mục (file này thuộc phạm vi của bạn). Trạng thái chính thức của từng sản phẩm nằm ở **đầu file sản phẩm**.

## Các bước làm

- [x] Nhận danh sách UC và FR từ Nguyên
- [ ] Đặc tả **8 UC: UC02, UC10, UC11, UC13, UC14, UC17, UC21, UC22** – mỗi UC 1 file trong `docs/03_UseCase/specs/` (Đã hoàn thành: UC02, UC10, UC11, UC13)
- [ ] Mỗi UC nêu ít nhất 2 luồng thay thế/ngoại lệ
- [ ] Vẽ **4 Sequence Diagram** SD1–SD4 → `docs/04_ThietKe/02_sequence/`; tên thông điệp khớp tên phương thức trong Class Diagram của Quang
- [ ] Đối chiếu luồng với Activity Diagram của Nguyên; sai khác thì báo Nguyên sửa
- [ ] Viết 3–5 dòng giải thích dưới mỗi sơ đồ

## Output cuối cùng

- [ ] 8 bảng đặc tả Use Case (UC02, 10, 11, 13, 14, 17, 21, 22)
- [ ] 4 Sequence Diagram (SD1–SD4) + 3–5 dòng giải thích mỗi sơ đồ

## Phần của mình nối với ai (trả lời câu 3 khi báo cáo)

- **Nguyên:** nhận FR/UC; đối chiếu SD với AD1, AD2.
- **Quang:** tên thông điệp trong SD phải khớp tên phương thức trong Class Diagram (mỗi file SD có bảng đối chiếu).

## Khái niệm cần giải thích được

- **Tiền điều kiện / hậu điều kiện:** điều kiện cần có trước khi chạy UC / trạng thái hệ thống sau khi UC xong
- **Luồng chính – luồng thay thế – ngoại lệ:** đường đi bình thường / đường rẽ hợp lệ / tình huống lỗi
- **Sequence Diagram:** thứ tự thông điệp theo thời gian giữa các đối tượng; lifeline, activation bar, message gửi và phản hồi
- **Combined fragment:** alt (rẽ nhánh), opt (tùy chọn), loop (lặp)
- **Boundary – Controller – Entity:** phân vai đối tượng (giao diện – điều phối – dữ liệu)
- **Sequence vs Activity:** Sequence = ai gửi gì cho ai theo thời gian; Activity = các bước công việc và rẽ nhánh

## Trọng tâm khi báo cáo

**Mỗi người phải trả lời được 3 câu:** (1) Phần mình làm gì? (2) Vì sao làm như vậy? (3) Phần mình nối với phần của ai?

- Luồng UC13: kiểm tồn → tính ship → tạo đơn → trừ tồn → thanh toán; xử lý ngoại lệ khi hết hàng/thanh toán lỗi.
- Đọc được 1 Sequence từ trên xuống; SD4 minh họa việc Carrier gọi ngược về hệ thống (callback).
