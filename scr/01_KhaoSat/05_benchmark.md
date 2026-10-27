# Benchmark website đối thủ

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

## 1. Danh sách website so sánh

| Website | Loại | Đối tượng | Link |
| --- | --- | --- | --- |
| **MyKingdom** | Chuyên đồ chơi, chuỗi lớn | Cha mẹ, em bé 0-12 tuổi | https://mykingdom.com.vn |
| **Con Cưng** | Chuyên đồ chơi, chuỗi lớn | Cha mẹ, em bé 0-12 tuổi | https://concung.com |
| **Shopee Mall (Đồ chơi)** | Sàn TMĐT, nhiều seller | Đại chúng | https://shopee.vn/mall/%C4%91%C6%A1-ch%C6%A1i-c.16095 |

> **Tiêu chí chọn:** Các website đại diện cho 3 mô hình: (1) Chuỗi chuyên ngành đồ chơi lớn (MyKingdom, Con Cưng), (2) Sàn TMĐT phổ biến nhất VN (Shopee).

---

## 2. Bảng so sánh chi tiết

| Tiêu chí | **MyKingdom** | **Con Cưng** | **Shopee Mall** |
| --- | --- | --- | --- |
| **Bộ lọc sản phẩm** | Danh mục, Thương hiệu, Độ tuổi, Giá, Chất liệu, Giới tính, Khuyến mãi | Danh mục, Thương hiệu, Độ tuổi, Giá, Chất liệu, Đặc điểm (giáo dục, vận động...) | Danh mục, Thương hiệu, Giá, Khuyến mãi, Đánh giá, Vận chuyển, Vị trí shop |
| **Giỏ hàng** | Giỏ hàng kéo (slide-out), cập nhật real-time, lưu khi chưa đăng nhập | Giỏ hàng trang riêng, cập nhật real-time, lưu khi chưa đăng nhập | Giỏ hàng trang riêng, slide-out mobile, lưu vĩnh viễn (cookie), gợi ý combo |
| **Số bước checkout** | 4 bước: Giỏ hàng → Thông tin giao hàng → Thanh toán → Xác nhận | 3 bước: Giỏ hàng → Thông tin giao hàng & thanh toán → Xác nhận | 1 trang (single-page): Địa chỉ, ship, thanh toán, voucher gộp chung |
| **Thanh toán** | COD, Chuyển khoản, VNPAY, MoMo, Thẻ, Trả góp | COD, Chuyển khoản, VNPAY, MoMo, Thẻ, Trả góp | COD, ShopeePay, Thẻ, Ví, Trả góp, Voucher sàn |
| **Theo dõi đơn** | Trang "Đơn hàng của tôi": timeline chi tiết, mã vận đơn link GHN/GHTK, hủy đơn tự助 | Trang "Đơn mua": timeline, mã vận đơn, hủy đơn, đổi trả | Trang "Đơn mua": timeline real-time, map shipper, chat với shipper, hủy/đổi trả |
| **Đặc điểm nổi bật** | - Chuyên sâu độ tuổi, phát triển | - Bộ lọc theo phát triển kỹ năng | - UX checkout tối giản (1 trang) |
| | - Blog tư vấn mua sắm | - Chương trình thành viên CungPoint | - Voucher sàn + shop tự động |
| | - Chương trình thành viên MyPoint | - Cam kết chính hãng 111% | - Map shipper real-time |
| | | | - Hệ sinh thái voucher/flash sale |
| **Điểm yếu / Khó khăn cho shop nhỏ** | - Quá phức tạp, nhiều tính năng không cần | - Tương tự MyKingdom | - Phí sàn cao (10-15%) |
| | - Chi phí vận hành cao | - Phụ thuộc hệ thống nội bộ | - Khó kiểm soát trải nghiệm |
| | - Yêu cầu tích hợp ERP lớn | | - Cạnh tranh giá gay gắt |

---

## 3. Tính năng rút ra cho hệ thống của nhóm

| STT | Tính năng | Nguồn benchmark | Mức độ | Ghi chú |
| --- | --- | --- | --- | --- |
| 1 | **Bộ lọc đa tiêu chí:** Danh mục, Thương hiệu, Độ tuổi, Giá, Chất liệu/An toàn | MyKingdom, Con Cưng | **Must** | Core UX duyệt sản phẩm |
| 2 | **Giỏ hàng slide-out / persistent:** Cập nhật real-time, lưu localStorage khi chưa login, đồng bộ khi login | MyKingdom, Con Cưng, Shopee | **Must** | FR-02, FR-03 |
| 3 | **Checkout 1 trang (single-page):** Gộp địa chỉ, ship, thanh toán, coupon | Shopee | **Must** | FR-06, UC-08 |
| 4 | **Đa phương thức TT:** COD, Chuyển khoản, Cổng thanh toán (mô phỏng VNPAY/MoMo) | Tất cả 3 | **Must** | FR-11, UC-10 |
| 5 | **Coupon/Voucher:** Mã giảm giá %/cố định, điều kiện đơn tối thiểu, giới hạn lượt, stack/không stack | Shopee (voucher sàn+shop), MyKingdom/Con Cưng (mã giảm giá) | **Must** | FR-08, FR-21, BR-11 |
| 6 | **Tra cứu đơn có timeline:** Trạng thái rõ ràng (Chờ xác nhận → Đóng gói → Đang giao → Hoàn tất), mã vận đơn link hãng ship | Tất cả 3 | **Must** | FR-09, FR-10, UC-09 |
| 7 | **Mã vận đơn & tracking:** Sinh mã tự động, link sang trang hãng ship (mô phỏng) | Shopee (map real-time), MyKingdom/Con Cưng (link GHN/GHTK) | **Must** | FR-15, FR-16, FR-31, UC-12, UC-20 |
| 8 | **Hiển thị tồn kho real-time:** Badge "Chỉ còn X sản phẩm", "Hết hàng" | MyKingdom, Con Cưng, Shopee | **Should** | FR-01, FR-07, BR-01, BR-02 |
| 9 | **Phân quyền nhân viên:** Admin / Bán hàng / Kho / Kế toán (RBAC) | Hệ thống nội bộ chuỗi lớn | **Should** | FR-26, FR-27, UC-18 |
| 10 | **Báo cáo Dashboard:** Doanh thu, đơn, AOV, Top SP, tồn kho thấp, xuất Excel/PDF | Hệ thống nội bộ chuỗi lớn | **Should** | FR-23, FR-24, FR-25, UC-17 |
| 11 | **Quản lý coupon/phí ship cấu hình:** Rule-based, chủ shop tự cấu hình | Hệ thống nội bộ chuỗi lớn | **Should** | FR-21, FR-22, UC-16 |
| 12 | **Xử lý ngoại lệ có quy trình:** Phân loại lỗi, hành động chuẩn, audit log | Hệ thống nội bộ chuỗi lớn | **Could** | FR-17, FR-18, BR-09, NFR-05, UC-13 |
| 13 | **Blog/Tư vấn mua sắm** | MyKingdom, Con Cưng | **Won't (Phase 2)** | Out-of-scope hiện tại |
| 14 | **Chương trình thành viên/Điểm thưởng** | MyKingdom (MyPoint), Con Cưng (CungPoint) | **Won't (Phase 2)** | Out-of-scope hiện tại |
| 15 | **Map shipper real-time / Chat shipper** | Shopee | **Won't (Phase 2)** | Yêu cầu tích hợp API thật |
| 16 | **Đổi trả/hoàn tiền tự động** | Shopee, MyKingdom | **Won't (Phase 2)** | Out-of-scope hiện tại |

---

## 4. Kết luận benchmark

**Xu hướng chung (Must-have cho shop nhỏ):**
1. **Checkout 1 trang** là chuẩn mực (Shopee) – giảm bỏ giỏ, tăng chuyển đổi.
2. **Bộ lọc theo độ tuổi + an toàn** là đặc thú ngành đồ chơi (MyKingdom, Con Cưng) – bắt buộc có.
3. **Timeline đơn hàng + mã vận đơn** là kỳ vọng cơ bản của khách – không có = kém chuyên nghiệp.
4. **COD vẫn là king** ở VN – phải hỗ trợ tốt, giảm rủi ro từ chối đơn.
5. **Phí ship minh bạch, ngưỡng miễn phí rõ ràng** – giảm abandon cart ở bước checkout.

**Chiến lược khác biệt cho shop nhỏ:**
- Không copy tất cả feature của chuỗi lớn → tập trung **core loop**: Duyệt → Giỏ → Checkout → Đơn → Tracking.
- **Mô phỏng tích hợp** (thanh toán, shipper) đủ để demo, không tốn chi phí KYC/hợp đồng.
- **Cấu hình rule-based** (coupon, phí ship, ngưỡng) – chủ shop tự điều chỉnh không cần dev.
- **Phân quyền nhẹ** (4 vai trò) – đủ cho team 3-5 người.
- **Audit log** – bắt buộc cho minh bạch, tranh chấp.

**Hành động tiếp theo:**
- [ ] Chụp màn hình các màn hình quan trọng của 3 web (Home, PLP, PDP, Cart, Checkout, Order Detail, Tracking) → lưu `hinh/benchmark_*.png`.
- [ ] Điền bảng so sánh chi tiết hơn (UI components, micro-interactions, error states).
- [ ] Cập nhật file `06_nhu_cau_yeu_cau.md` nếu phát hiện yêu cầu mới từ benchmark.

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

## 1. Danh sách website so sánh

| Website | Loại | Đối tượng | Link |
| --- | --- | --- | --- |
| **MyKingdom** | Chuyên đồ chơi, chuỗi lớn | Cha mẹ, em bé 0-12 tuổi | https://mykingdom.com.vn |
| **Con Cưng** | Chuyên đồ chơi, chuỗi lớn | Cha mẹ, em bé 0-12 tuổi | https://concung.com |
| **Shopee Mall (Đồ chơi)** | Sàn TMĐT, nhiều seller | Đại chúng | https://shopee.vn/mall/%C4%91%C6%A1-ch%C6%A1i-c.16095 |

> **Tiêu chí chọn:** Các website đại diện cho 3 mô hình: (1) Chuỗi chuyên ngành đồ chơi lớn (MyKingdom, Con Cưng), (2) Sàn TMĐT phổ biến nhất VN (Shopee).

---

## 2. Bảng so sánh chi tiết

| Tiêu chí | **MyKingdom** | **Con Cưng** | **Shopee Mall** |
| --- | --- | --- | --- |
| **Bộ lọc sản phẩm** | Danh mục, Thương hiệu, Độ tuổi, Giá, Chất liệu, Giới tính, Khuyến mãi | Danh mục, Thương hiệu, Độ tuổi, Giá, Chất liệu, Đặc điểm (giáo dục, vận động...) | Danh mục, Thương hiệu, Giá, Khuyến mãi, Đánh giá, Vận chuyển, Vị trí shop |
| **Giỏ hàng** | Giỏ hàng kéo (slide-out), cập nhật real-time, lưu khi chưa đăng nhập | Giỏ hàng trang riêng, cập nhật real-time, lưu khi chưa đăng nhập | Giỏ hàng trang riêng, slide-out mobile, lưu vĩnh viễn (cookie), gợi ý combo |
| **Số bước checkout** | 4 bước: Giỏ hàng → Thông tin giao hàng → Thanh toán → Xác nhận | 3 bước: Giỏ hàng → Thông tin giao hàng & thanh toán → Xác nhận | 1 trang (single-page): Địa chỉ, ship, thanh toán, voucher gộp chung |
| **Thanh toán** | COD, Chuyển khoản, VNPAY, MoMo, Thẻ, Trả góp | COD, Chuyển khoản, VNPAY, MoMo, Thẻ, Trả góp | COD, ShopeePay, Thẻ, Ví, Trả góp, Voucher sàn |
| **Theo dõi đơn** | Trang "Đơn hàng của tôi": timeline chi tiết, mã vận đơn link GHN/GHTK, hủy đơn tự助 | Trang "Đơn mua": timeline, mã vận đơn, hủy đơn, đổi trả | Trang "Đơn mua": timeline real-time, map shipper, chat với shipper, hủy/đổi trả |
| **Đặc điểm nổi bật** | - Chuyên sâu độ tuổi, phát triển | - Bộ lọc theo phát triển kỹ năng | - UX checkout tối giản (1 trang) |
| | - Blog tư vấn mua sắm | - Chương trình thành viên CungPoint | - Voucher sàn + shop tự động |
| | - Chương trình thành viên MyPoint | - Cam kết chính hãng 111% | - Map shipper real-time |
| | | | - Hệ sinh thái voucher/flash sale |
| **Điểm yếu / Khó khăn cho shop nhỏ** | - Quá phức tạp, nhiều tính năng không cần | - Tương tự MyKingdom | - Phí sàn cao (10-15%) |
| | - Chi phí vận hành cao | - Phụ thuộc hệ thống nội bộ | - Khó kiểm soát trải nghiệm |
| | - Yêu cầu tích hợp ERP lớn | | - Cạnh tranh giá gay gắt |

---

## 3. Tính năng rút ra cho hệ thống của nhóm

| STT | Tính năng | Nguồn benchmark | Mức độ | Ghi chú |
| --- | --- | --- | --- | --- |
| 1 | **Bộ lọc đa tiêu chí:** Danh mục, Thương hiệu, Độ tuổi, Giá, Chất liệu/An toàn | MyKingdom, Con Cưng | **Must** | Core UX duyệt sản phẩm |
| 2 | **Giỏ hàng slide-out / persistent:** Cập nhật real-time, lưu localStorage khi chưa login, đồng bộ khi login | MyKingdom, Con Cưng, Shopee | **Must** | FR-02, FR-03 |
| 3 | **Checkout 1 trang (single-page):** Gộp địa chỉ, ship, thanh toán, coupon | Shopee | **Must** | FR-06, UC-08 |
| 4 | **Đa phương thức TT:** COD, Chuyển khoản, Cổng thanh toán (mô phỏng VNPAY/MoMo) | Tất cả 3 | **Must** | FR-11, UC-10 |
| 5 | **Coupon/Voucher:** Mã giảm giá %/cố định, điều kiện đơn tối thiểu, giới hạn lượt, stack/không stack | Shopee (voucher sàn+shop), MyKingdom/Con Cưng (mã giảm giá) | **Must** | FR-08, FR-21, BR-11 |
| 6 | **Tra cứu đơn có timeline:** Trạng thái rõ ràng (Chờ xác nhận → Đóng gói → Đang giao → Hoàn tất), mã vận đơn link hãng ship | Tất cả 3 | **Must** | FR-09, FR-10, UC-09 |
| 7 | **Mã vận đơn & tracking:** Sinh mã tự động, link sang trang hãng ship (mô phỏng) | Shopee (map real-time), MyKingdom/Con Cưng (link GHN/GHTK) | **Must** | FR-15, FR-16, FR-31, UC-12, UC-20 |
| 8 | **Hiển thị tồn kho real-time:** Badge "Chỉ còn X sản phẩm", "Hết hàng" | MyKingdom, Con Cưng, Shopee | **Should** | FR-01, FR-07, BR-01, BR-02 |
| 9 | **Phân quyền nhân viên:** Admin / Bán hàng / Kho / Kế toán (RBAC) | Hệ thống nội bộ chuỗi lớn | **Should** | FR-26, FR-27, UC-18 |
| 10 | **Báo cáo Dashboard:** Doanh thu, đơn, AOV, Top SP, tồn kho thấp, xuất Excel/PDF | Hệ thống nội bộ chuỗi lớn | **Should** | FR-23, FR-24, FR-25, UC-17 |
| 11 | **Quản lý coupon/phí ship cấu hình:** Rule-based, chủ shop tự cấu hình | Hệ thống nội bộ chuỗi lớn | **Should** | FR-21, FR-22, UC-16 |
| 12 | **Xử lý ngoại lệ có quy trình:** Phân loại lỗi, hành động chuẩn, audit log | Hệ thống nội bộ chuỗi lớn | **Could** | FR-17, FR-18, BR-09, NFR-05, UC-13 |
| 13 | **Blog/Tư vấn mua sắm** | MyKingdom, Con Cưng | **Won't (Phase 2)** | Out-of-scope hiện tại |
| 14 | **Chương trình thành viên/Điểm thưởng** | MyKingdom (MyPoint), Con Cưng (CungPoint) | **Won't (Phase 2)** | Out-of-scope hiện tại |
| 15 | **Map shipper real-time / Chat shipper** | Shopee | **Won't (Phase 2)** | Yêu cầu tích hợp API thật |
| 16 | **Đổi trả/hoàn tiền tự động** | Shopee, MyKingdom | **Won't (Phase 2)** | Out-of-scope hiện tại |

---

## 4. Kết luận benchmark

**Xu hướng chung (Must-have cho shop nhỏ):**
1. **Checkout 1 trang** là chuẩn mực (Shopee) – giảm bỏ giỏ, tăng chuyển đổi.
2. **Bộ lọc theo độ tuổi + an toàn** là đặc thú ngành đồ chơi (MyKingdom, Con Cưng) – bắt buộc có.
3. **Timeline đơn hàng + mã vận đơn** là kỳ vọng cơ bản của khách – không có = kém chuyên nghiệp.
4. **COD vẫn là king** ở VN – phải hỗ trợ tốt, giảm rủi ro từ chối đơn.
5. **Phí ship minh bạch, ngưỡng miễn phí rõ ràng** – giảm abandon cart ở bước checkout.

**Chiến lược khác biệt cho shop nhỏ:**
- Không copy tất cả feature của chuỗi lớn → tập trung **core loop**: Duyệt → Giỏ → Checkout → Đơn → Tracking.
- **Mô phỏng tích hợp** (thanh toán, shipper) đủ để demo, không tốn chi phí KYC/hợp đồng.
- **Cấu hình rule-based** (coupon, phí ship, ngưỡng) – chủ shop tự điều chỉnh không cần dev.
- **Phân quyền nhẹ** (4 vai trò) – đủ cho team 3-5 người.
- **Audit log** – bắt buộc cho minh bạch, tranh chấp.

**Hành động tiếp theo:**
- [ ] Chụp màn hình các màn hình quan trọng của 3 web (Home, PLP, PDP, Cart, Checkout, Order Detail, Tracking) → lưu `hinh/benchmark_*.png`.
- [ ] Điền bảng so sánh chi tiết hơn (UI components, micro-interactions, error states).
- [ ] Cập nhật file `06_nhu_cau_yeu_cau.md` nếu phát hiện yêu cầu mới từ benchmark. 
