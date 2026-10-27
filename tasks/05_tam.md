# Phan Trí Tâm – 52300253 (Thiết kế giao diện)

**Phụ trách:** UI phía khách hàng (8 màn hình) • Prototype luồng mua hàng • Video demo

> Tick `[x]` khi xong từng mục (file này thuộc phạm vi của bạn). Trạng thái chính thức của từng sản phẩm nằm ở **đầu file sản phẩm**.

## Các bước làm

- [ ] Phác **wireframe low-fi** 8 màn khách hàng (giấy hoặc Figma) theo bảng trong `docs/05_UI/01_man_hinh_khach_hang.md`
- [ ] Chọn mini design system: 1 bảng màu (tươi sáng, thân thiện), 1 font, component dùng lại (nút, thẻ sản phẩm, thanh menu, nhãn trạng thái đơn)
- [ ] Thiết kế **hi-fi 8 màn** trên Figma (khung điện thoại; thêm desktop nếu kịp); mỗi màn ghi rõ tên màn + mã UC
- [ ] Làm thêm trạng thái đặc biệt: giỏ trống, hết hàng, thanh toán thất bại
- [ ] Nối **prototype click-through**: Trang chủ → Danh sách → Chi tiết → Giỏ → Checkout → Thành công → Theo dõi đơn
- [ ] Gộp file Figma với phần admin của Phương thành 1 link prototype (cài quyền xem); quay **video demo 1–2 phút** (để trên Drive, dán link)
- [ ] Viết bảng *Màn hình ↔ Use Case ↔ Yêu cầu* (truy vết) → `04_truy_vet_man_hinh_uc_yeu_cau.md`

## Output cuối cùng

- [ ] 8 màn hình hi-fi (kèm trạng thái giỏ trống / hết hàng / thanh toán lỗi)
- [ ] Link prototype Figma bấm được (gộp cả 5 màn admin của Phương) + video demo 1–2 phút
- [ ] Bảng *Màn hình ↔ Use Case ↔ Yêu cầu*

## Phần của mình nối với ai (trả lời câu 3 khi báo cáo)

- **Phương:** gộp prototype admin (A1–A5) và khách (C1–C8) thành 1 link.
- **Quang:** xin trường dữ liệu cho màn hình (ERD).
- **Nguyên, Việt:** màn hình bám UC và luồng đã đặc tả.

## Khái niệm cần giải thích được

- **Wireframe – Mockup – Prototype:** khung sườn – bản thiết kế hoàn chỉnh – bản bấm thử được
- **Low-fi vs Hi-fi:** bản phác đơn giản / bản gần giống sản phẩm thật (màu, ảnh, chữ)
- **UI vs UX:** giao diện nhìn thấy / trải nghiệm khi dùng
- **User flow:** đường đi của người dùng để hoàn thành mục tiêu (mua hàng)
- **Nguyên tắc thiết kế:** nhất quán, phản hồi rõ ràng, giảm số bước, dễ đọc trên điện thoại (responsive)
- **Design system / component:** bộ màu, font, thành phần dùng lại

## Trọng tâm khi báo cáo

**Mỗi người phải trả lời được 3 câu:** (1) Phần mình làm gì? (2) Vì sao làm như vậy? (3) Phần mình nối với phần của ai?

- Luồng mua hàng ngắn gọn (≤ 3 bước checkout) và vì sao thiết kế như vậy.
- Mỗi màn hình bám đúng 1 UC (truy vết), có xử lý trạng thái lỗi/trống.
- Demo prototype bấm được từ trang chủ đến theo dõi đơn.
