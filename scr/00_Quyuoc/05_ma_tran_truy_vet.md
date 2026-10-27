# Ma trận truy vết

Dùng để đối soát chéo giữa 5 người. Khi một người đổi mã/tên, phải đối chiếu lại cột tương ứng.

| Chức năng | Use Case | Activity / Sequence | Màn hình | Bảng dữ liệu |
| --- | --- | --- | --- | --- |
| Giỏ hàng | UC10, UC11 | SD1 | C3, C4 | CART, CART_ITEM, PRODUCT |
| Đặt hàng | UC13, UC12 | AD1, SD2 | C5, C6 | ORDERS, ORDER_ITEM, COUPON, ADDRESS |
| Thanh toán | UC14 | AD1, SD2 | C5 | PAYMENT |
| Xử lý đơn | UC17, UC18 | AD2, SD3 | A3, A4 | ORDERS, INVENTORY_LOG |
| Vận chuyển | UC20, UC21, UC22 | AD2, SD3, SD4 | A4, C7 | SHIPMENT, CARRIER |
| Theo dõi đơn | UC15, UC16 | – | C7 | ORDERS |

## Các mối nối bàn giao giữa thành viên

| Từ | Đến | Bàn giao | Hạn trong kế hoạch |
| --- | --- | --- | --- |
| Phương | Nguyên | Bảng *Nhu cầu → Yêu cầu* (`01_KhaoSat/06_...`) | Cuối tuần 1 |
| Nguyên | Việt, Quang | FR và danh sách UC cuối | Cuối tuần 1 |
| Việt | Quang | Tên thông điệp SD ↔ phương thức Class Diagram | Khi vẽ SD |
| Việt | Nguyên | Sai khác giữa Sequence và Activity | Khi đối chiếu |
| Quang | Phương, Tâm | ERD / trường dữ liệu cho màn hình | Khi cần |
| Tâm | Phương | Gộp prototype admin + khách thành 1 link Figma | Cuối |
