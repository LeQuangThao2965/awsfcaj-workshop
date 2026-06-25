---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Điều chỉnh và tối ưu cấu trúc dữ liệu để hỗ trợ chức năng thanh toán và giao dịch.
* Bổ sung các ràng buộc và thuộc tính cần thiết cho mô hình marketplace.
* Tích hợp SePay để thử nghiệm quy trình thanh toán trực tuyến.
* Chuẩn bị logic xác nhận giao dịch theo thời gian thực.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Rà soát cấu trúc database hiện tại trước khi thêm chức năng thanh toán. <br>- Bổ sung hoặc điều chỉnh các field như mã đơn hàng, số tiền giao dịch, trạng thái, buyer, seller và sản phẩm đã mua. <br>- Định nghĩa ràng buộc để hạn chế dữ liệu thanh toán không hợp lệ. | 08/06/2026 | 08/06/2026 | |
| 3 | - Cập nhật model backend và service logic để hỗ trợ purchase và transaction records. <br>- Chuẩn bị API cho tạo đơn hàng và kiểm tra trạng thái thanh toán. <br>- Kiểm thử cấu trúc dữ liệu mới bằng dữ liệu thanh toán mẫu. | 09/06/2026 | 09/06/2026 | |
| 4 | - Nghiên cứu SePay API và quy trình webhook/thông báo thanh toán. <br>- Chuẩn bị phương án tích hợp để thử nghiệm thanh toán. <br>- Cấu hình endpoint backend cần thiết để nhận hoặc xử lý thông tin giao dịch. | 10/06/2026 | 10/06/2026 | |
| 5 | - Tích hợp SePay vào dự án để thử nghiệm thanh toán. <br>- Xây dựng logic đối chiếu thông tin thanh toán với đơn hàng trong hệ thống. <br>- Kiểm thử luồng xác nhận giao dịch bằng dữ liệu mẫu. | 11/06/2026 | 11/06/2026 | |
| 6 | - Rà soát quy trình thanh toán và sửa các lỗi cơ bản. <br>- Ghi nhận các giới hạn còn lại như seller registration chưa hoàn chỉnh, category management còn thiếu và preview tài liệu chưa ổn định. <br>- Chuẩn bị hệ thống cho giai đoạn kiểm thử chức năng cốt lõi ở tuần tiếp theo. | 12/06/2026 | 12/06/2026 | |


### Kết quả đạt được tuần 8:

* Cập nhật cấu trúc dữ liệu với các trường và ràng buộc cho products, orders, transactions và user roles.
* Chuẩn bị workflow thanh toán gồm tạo đơn hàng, trạng thái giao dịch và xác nhận mua hàng.
* Tích hợp SePay API để thử nghiệm thanh toán và truy vấn giao dịch.
* Xây dựng logic ban đầu để xử lý thông báo giao dịch và cập nhật trạng thái thanh toán.
* Xác định các vấn đề còn lại liên quan đến đăng ký seller, quản lý danh mục và xem trước tài liệu.
