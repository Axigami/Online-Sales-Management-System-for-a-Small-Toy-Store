# Tô Đoàn Khánh Nguyên – 52300231 (Phân tích yêu cầu)

**Phụ trách:** Đặc tả yêu cầu chức năng (FR) & phi chức năng (NFR) • Use Case Diagram • Activity Diagram

> Tick `[x]` khi xong từng mục (file này thuộc phạm vi của bạn). Trạng thái chính thức của từng sản phẩm nằm ở **đầu file sản phẩm**.

## Các bước làm

- [ ] Nhận bảng *Nhu cầu → Yêu cầu* từ Phương → viết thành FR theo mẫu *Hệ thống cho phép [actor] [làm gì]*, đánh mã FR01, FR02… → `docs/02_YeuCau/01_yeu_cau_chuc_nang.md`
- [ ] Gán độ ưu tiên **MoSCoW** (Must / Should / Could) và gắn mã UC cho từng FR
- [ ] Viết NFR theo 8 nhóm – mỗi dòng có chỉ số đo được → `02_yeu_cau_phi_chuc_nang.md`
- [ ] Vẽ **Use Case Diagram**: 1 sơ đồ tổng quát (6 actor) + 3 sơ đồ module: (a) Mua hàng UC05–UC16; (b) Xử lý đơn & vận chuyển UC17–UC22; (c) Quản trị UC07–09, UC23–25 → `docs/03_UseCase/01_use_case_diagram.md`
- [ ] Thể hiện đủ quan hệ đã chốt: Customer → Guest, Admin → Staff; UC13 «include» UC02, UC20; UC13 «extend» UC12, UC14
- [ ] Vẽ **2 Activity Diagram có swimlane** (AD1, AD2) → `docs/04_ThietKe/01_activity/`
- [ ] Gửi bản FR/UC cuối cho Việt và Quang **cuối tuần 1**

## Output cuối cùng

- [ ] Bảng FR (khoảng 25–30 dòng, có mã, ưu tiên, UC) và bảng NFR (8 dòng)
- [ ] 4 Use Case Diagram (1 tổng quát + 3 module) và bảng danh sách UC
- [ ] 2 Activity Diagram có swimlane + 3–5 dòng giải thích mỗi sơ đồ

## Phần của mình nối với ai (trả lời câu 3 khi báo cáo)

- **Phương:** nhận bảng *Nhu cầu → Yêu cầu* làm đầu vào.
- **Việt:** nhận FR + danh sách UC để đặc tả chi tiết; đối chiếu SD với AD của bạn.
- **Quang:** nhận FR/UC để chốt thực thể và lớp.

## Khái niệm cần giải thích được

- **FR vs NFR:** FR = hệ thống làm gì (đặt hàng); NFR = làm tốt đến đâu (tải ≤ 3 giây, bảo mật)
- **Actor:** đối tượng bên ngoài tương tác với hệ thống (người hoặc hệ thống ngoài), không phải một người cụ thể
- **Use Case:** một chức năng mang lại giá trị cho actor
- **Include / Extend / Generalization:** include = bắt buộc đi kèm (Đặt hàng → Tính phí ship); extend = tùy chọn, có điều kiện (Đặt hàng → Thanh toán online / Áp mã); generalization = kế thừa (Customer → Guest)
- **MoSCoW:** Must / Should / Could / Won't – xếp ưu tiên yêu cầu
- **Activity Diagram:** mô tả luồng công việc; swimlane (ai làm), decision (rẽ nhánh), fork/join (song song)

## Trọng tâm khi báo cáo

**Mỗi người phải trả lời được 3 câu:** (1) Phần mình làm gì? (2) Vì sao làm như vậy? (3) Phần mình nối với phần của ai?

- Vì sao có 6 actor (4 người + 2 hệ thống ngoài); phân biệt include/extend bằng ví dụ ngay trong đề tài.
- NFR có chỉ số đo được, không viết chung chung.
- Đi được luồng đặt hàng và xử lý đơn từ đầu đến cuối trên Activity Diagram.
