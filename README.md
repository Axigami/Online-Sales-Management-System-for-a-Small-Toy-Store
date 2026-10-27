# Nhóm 5 – Hệ thống quản lý bán hàng trực tuyến cho cửa hàng đồ chơi nhỏ

*(giỏ hàng – đơn hàng – vận chuyển)*

Repo này chứa **toàn bộ tài liệu phân tích – thiết kế** của đồ án. Mỗi thành viên làm việc trong thư mục của mình, nhóm trưởng duyệt rồi mới gộp vào `main`.

## Thành viên và phạm vi

| Thành viên | MSSV | Vai trò | Thư mục / file phụ trách |
| --- | --- | --- | --- |
| Nguyễn Đức Phương | 52300244 | Nhóm trưởng | `docs/01_KhaoSat/`, `docs/05_UI/02_man_hinh_admin.md`, `docs/06_BaoCao/` |
| Tô Đoàn Khánh Nguyên | 52300231 | Phân tích yêu cầu | `docs/02_YeuCau/`, `docs/03_UseCase/01_use_case_diagram.md`, `docs/04_ThietKe/01_activity/` |
| Huỳnh Nguyễn Quốc Việt | 52300267 | Phân tích hành vi | `docs/03_UseCase/specs/`, `docs/04_ThietKe/02_sequence/` |
| Trần Vinh Quang | 52300248 | Thiết kế dữ liệu | `docs/04_ThietKe/03_class_diagram.md`, `04_erd.md`, `05_tu_dien_du_lieu.md` |
| Phan Trí Tâm | 52300253 | Thiết kế giao diện | `docs/05_UI/01_man_hinh_khach_hang.md`, `03_prototype_va_video.md`, `04_truy_vet_...md` |

Hướng dẫn chi tiết việc của từng người nằm trong thư mục [`tasks/`](tasks/).

## Cấu trúc repo

```text
.
├── docs/
│   ├── 00_QuyUoc/      Quy ước chung: phạm vi, actor, danh sách UC, trạng thái, BR, mã, ma trận truy vết
│   ├── 01_KhaoSat/     Chương 1: khảo sát, As-Is, phỏng vấn, bảng hỏi, benchmark
│   ├── 02_YeuCau/      FR, NFR
│   ├── 03_UseCase/     Use Case Diagram + 8 bảng đặc tả UC
│   ├── 04_ThietKe/     Activity, Sequence, Class Diagram, ERD, từ điển dữ liệu
│   ├── 05_UI/          Màn hình khách hàng (C1–C8), admin (A1–A5), prototype, truy vết
│   └── 06_BaoCao/      Mục lục báo cáo, chương 7, theo dõi báo cáo + slide
├── tasks/              Hướng dẫn + checklist từng thành viên
├── scripts/            Công cụ kiểm tra / tiến độ / build báo cáo
├── team.json           Danh sách thành viên + phạm vi được sửa
└── .github/            CODEOWNERS, template PR/Issue, GitHub Actions
```
