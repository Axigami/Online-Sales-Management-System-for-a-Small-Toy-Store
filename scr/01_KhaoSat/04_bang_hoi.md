# Bảng hỏi phụ huynh (Khách hàng mục tiêu)

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

- **Link Google Form:** [https://forms.gle/xxx] 
- **Số người trả lời:** [Điền sau khi thu thập, mục tiêu 15-20 phụ huynh] 
- **Thời gian mở:** [DD/MM/YYYY] – [DD/MM/YYYY] 

## Danh sách câu hỏi

| # | Câu hỏi | Loại | Mục đích |
| --- | --- | --- | --- |
| 1 | **Bạn thường mua đồ chơi cho con qua kênh nào?** | Đa chọn | Xác định kênh mua hàng hiện tại, mức độ quen online |
| 2 | **Tiêu chí quan trọng nhất khi chọn mua đồ chơi?** | Đa chọn | Xác định bộ lọc & thông tin cần hiển thị trên web |
| 3 | **Bạn ngại điều gì nhất khi mua đồ chơi online?** | Đa chọn | Xác định pain points → tính năng ưu tiên (hình ảnh thật, tracking, đổi trả, thanh toán COD) |
| 4 | **Phương thức thanh toán bạn ưa thích?** | Đơn chọn | Xác định phương thức thanh toán cần hỗ trợ (COD, chuyển khoản, cổng mô phỏng) |
| 5 | **Bạn có muốn tự tra cứu đơn hàng trên website không?** | Đơn chọn | Xác thực nhu cầu UC-09 (Tra cứu & theo dõi đơn) |
| 6 | **Bạn thường mua đồ chơi cho độ tuổi nào?** | Đơn chọn | Xác định thuộc tính "Độ tuổi" cho bộ lọc sản phẩm |
| 7 | **Tần suất mua đồ chơi online bao nhiêu lần/tháng?** | Đơn chọn | Ước lượng tần suất truy cập, loyalty |
| 8 | **Mức giá trung bình một đơn hàng bạn thường chi?** | Đơn chọn | Xác định ngưỡng miễn phí ship, AOV dự kiến |

## Kết quả (1–2 biểu đồ)

> **Lưu ý:** Xuất biểu đồ từ Google Forms → lưu PNG vào thư mục `hinh/` → chèn vào dưới đây.

### Biểu đồ 1: Kênh mua hàng hiện tại
![Kênh mua hàng](hinh/bang_hoi_kenh_mua.png)

### Biểu đồ 2: Tiêu chí chọn mua
![Tiêu chí chọn mua](hinh/bang_hoi_tieu_chi.png)

### Biểu đồ 3: Nỗi ngại mua online
![Nỗi ngại mua online](hinh/bang_hoi_noi_ngai.png)

### Biểu đồ 4: Phương thức thanh toán ưa thích
![Phương thức thanh toán](hinh/bang_hoi_thanh_toan.png)

### Biểu đồ 5: Mong muốn tra cứu đơn
![Mong muốn tra cứu đơn](hinh/bang_hoi_tra_cuu.png)

## Nhận xét rút ra

1. **Kênh chủ đạo:** [Ví dụ: 70% mua qua Shopee/Facebook → web cần UX tương tự, dễ dùng như sàn TMĐT]
2. **Top 3 tiêu chí:** [Ví dụ: Giá, An toàn, Độ tuổi] → Bộ lọc web phải có 3 tiêu chí này lên đầu.
3. **Nỗi ngại lớn nhất:** [Ví dụ: Không như hình, giao chậm, phí ship cao] → Cần: hình ảnh thật/video, tracking real-time, cấu hình phí ship minh bạch, ngưỡng miễn phí.
4. **Thanh toán:** [Ví dụ: 80% chọn COD] → Ưu tiên COD, hỗ trợ chuyển khoản, cổng mô phỏng cho demo.
5. **Tra cứu đơn:** [Ví dụ: 90% muốn tự tra cứu] → UC-09 là Must-have, timeline chi tiết, mã vận đơn.
6. **Độ tuổi & Giá:** [Ví dụ: Phổ biến 1-6 tuổi, đơn 200-500k] → Danh mục SP tập trung độ tuổi này, ngưỡng miễn phí 500k phù hợp.

## Ma trận mapping sang yêu cầu hệ thống

| Câu hỏi | Nhu cầu rút ra | FR/UC liên quan (file 06) |
| --- | --- | --- |
| 1. Kênh mua | Web phải UX thân thiện, dễ dùng như sàn TMĐT | FR-01, FR-02, UC-01~04 |
| 2. Tiêu chí chọn | Bộ lọc: Giá, Độ tuổi, An toàn, Thương hiệu | FR-01, BR-01, UC-02 |
| 3. Nỗi ngại | Hình ảnh thật, tracking, phí ship minh bạch, COD | FR-03, FR-09, FR-10, FR-11, BR-04, BR-05, UC-08, UC-09 |
| 4. Thanh toán | COD, Chuyển khoản, Cổng mô phỏng | FR-11, UC-10 |
| 5. Tra cứu đơn | Trang "Đơn hàng của tôi", timeline, mã vận đơn | FR-09, FR-10, UC-09 |
| 6. Độ tuổi | Thuộc tính sản phẩm "Độ tuổi" | FR-19, BR-01 |
| 7. Tần suất | loyalty, tài khoản khách | FR-04, FR-05, UC-05, UC-07 |
| 8. Giá trung bình | Ngưỡng miễn phí ship, AOV | BR-04, FR-23 |

> **Phụ trách:** Nguyễn Đức Phương  
> **Trạng thái:** 🟡 Đang thực hiện  
> **Hạn:** —

---

- **Link Google Form:** [https://forms.gle/xxx] 
- **Số người trả lời:** [Điền sau khi thu thập, mục tiêu 15-20 phụ huynh] 
- **Thời gian mở:** [DD/MM/YYYY] – [DD/MM/YYYY] 

## Danh sách câu hỏi

| # | Câu hỏi | Loại | Mục đích |
| --- | --- | --- | --- |
| 1 | **Bạn thường mua đồ chơi cho con qua kênh nào?** | Đa chọn | Xác định kênh mua hàng hiện tại, mức độ quen online |
| 2 | **Tiêu chí quan trọng nhất khi chọn mua đồ chơi?** | Đa chọn | Xác định bộ lọc & thông tin cần hiển thị trên web |
| 3 | **Bạn ngại điều gì nhất khi mua đồ chơi online?** | Đa chọn | Xác định pain points → tính năng ưu tiên (hình ảnh thật, tracking, đổi trả, thanh toán COD) |
| 4 | **Phương thức thanh toán bạn ưa thích?** | Đơn chọn | Xác định phương thức thanh toán cần hỗ trợ (COD, chuyển khoản, cổng mô phỏng) |
| 5 | **Bạn có muốn tự tra cứu đơn hàng trên website không?** | Đơn chọn | Xác thực nhu cầu UC-09 (Tra cứu & theo dõi đơn) |
| 6 | **Bạn thường mua đồ chơi cho độ tuổi nào?** | Đơn chọn | Xác định thuộc tính "Độ tuổi" cho bộ lọc sản phẩm |
| 7 | **Tần suất mua đồ chơi online bao nhiêu lần/tháng?** | Đơn chọn | Ước lượng tần suất truy cập, loyalty |
| 8 | **Mức giá trung bình một đơn hàng bạn thường chi?** | Đơn chọn | Xác định ngưỡng miễn phí ship, AOV dự kiến |

## Kết quả (1–2 biểu đồ)

> **Lưu ý:** Xuất biểu đồ từ Google Forms → lưu PNG vào thư mục `hinh/` → chèn vào dưới đây.

### Biểu đồ 1: Kênh mua hàng hiện tại
![Kênh mua hàng](hinh/bang_hoi_kenh_mua.png)

### Biểu đồ 2: Tiêu chí chọn mua
![Tiêu chí chọn mua](hinh/bang_hoi_tieu_chi.png)

### Biểu đồ 3: Nỗi ngại mua online
![Nỗi ngại mua online](hinh/bang_hoi_noi_ngai.png)

### Biểu đồ 4: Phương thức thanh toán ưa thích
![Phương thức thanh toán](hinh/bang_hoi_thanh_toan.png)

### Biểu đồ 5: Mong muốn tra cứu đơn
![Mong muốn tra cứu đơn](hinh/bang_hoi_tra_cuu.png)

## Nhận xét rút ra

1. **Kênh chủ đạo:** [Ví dụ: 70% mua qua Shopee/Facebook → web cần UX tương tự, dễ dùng như sàn TMĐT]
2. **Top 3 tiêu chí:** [Ví dụ: Giá, An toàn, Độ tuổi] → Bộ lọc web phải có 3 tiêu chí này lên đầu.
3. **Nỗi ngại lớn nhất:** [Ví dụ: Không như hình, giao chậm, phí ship cao] → Cần: hình ảnh thật/video, tracking real-time, cấu hình phí ship minh bạch, ngưỡng miễn phí.
4. **Thanh toán:** [Ví dụ: 80% chọn COD] → Ưu tiên COD, hỗ trợ chuyển khoản, cổng mô phỏng cho demo.
5. **Tra cứu đơn:** [Ví dụ: 90% muốn tự tra cứu] → UC-09 là Must-have, timeline chi tiết, mã vận đơn.
6. **Độ tuổi & Giá:** [Ví dụ: Phổ biến 1-6 tuổi, đơn 200-500k] → Danh mục SP tập trung độ tuổi này, ngưỡng miễn phí 500k phù hợp.

## Ma trận mapping sang yêu cầu hệ thống

| Câu hỏi | Nhu cầu rút ra | FR/UC liên quan (file 06) |
| --- | --- | --- |
| 1. Kênh mua | Web phải UX thân thiện, dễ dùng như sàn TMĐT | FR-01, FR-02, UC-01~04 |
| 2. Tiêu chí chọn | Bộ lọc: Giá, Độ tuổi, An toàn, Thương hiệu | FR-01, BR-01, UC-02 |
| 3. Nỗi ngại | Hình ảnh thật, tracking, phí ship minh bạch, COD | FR-03, FR-09, FR-10, FR-11, BR-04, BR-05, UC-08, UC-09 |
| 4. Thanh toán | COD, Chuyển khoản, Cổng mô phỏng | FR-11, UC-10 |
| 5. Tra cứu đơn | Trang "Đơn hàng của tôi", timeline, mã vận đơn | FR-09, FR-10, UC-09 |
| 6. Độ tuổi | Thuộc tính sản phẩm "Độ tuổi" | FR-19, BR-01 |
| 7. Tần suất | loyalty, tài khoản khách | FR-04, FR-05, UC-05, UC-07 |
| 8. Giá trung bình | Ngưỡng miễn phí ship, AOV | BR-04, FR-23 | 
