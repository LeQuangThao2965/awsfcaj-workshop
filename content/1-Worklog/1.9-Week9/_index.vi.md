---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Hoàn thiện và kiểm thử các chức năng chính của prototype hiện tại.
* Cải thiện đăng ký/đăng nhập, tìm kiếm sản phẩm, quản lý người dùng, 3D model preview và xác nhận thanh toán.
* Rà soát các vấn đề còn tồn tại trước khi triển khai lên AWS.
* Lập kế hoạch giai đoạn tiếp theo cho vẽ architecture, deploy AWS, seller workflow và cải thiện preview tài liệu.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Rà soát prototype hiện tại và liệt kê các chức năng đã có. <br>- Kiểm thử luồng đăng ký, đăng nhập và xác thực. <br>- Kiểm tra hành vi session cơ bản và quyền truy cập. | 15/06/2026 | 15/06/2026 | |
| 3 | - Kiểm thử chức năng tìm kiếm và hiển thị sản phẩm. <br>- Kiểm tra trang chi tiết sản phẩm đối với tài liệu số và sản phẩm dạng 3D model. <br>- Kiểm tra hoạt động của 3D viewer đối với file GLB. | 16/06/2026 | 16/06/2026 | |
| 4 | - Kiểm thử chức năng quản lý người dùng của admin. <br>- Xác minh logic ban/unban user và ảnh hưởng đến quyền truy cập của user. <br>- Rà soát các phần còn thiếu trong phân quyền và đăng ký làm seller. | 17/06/2026 | 17/06/2026 | |
| 5 | - Kiểm thử luồng tích hợp thanh toán SePay. <br>- Gọi API SePay hoặc xử lý dữ liệu thông báo thanh toán để xác nhận giao dịch thành công. <br>- Kiểm tra việc cập nhật trạng thái mua hàng sau khi giao dịch thành công. | 18/06/2026 | 18/06/2026 | |
| 6 | - Rà soát các vấn đề còn lại và cập nhật kế hoạch dự án. <br>- Liệt kê các chức năng chưa hoàn thiện gồm deploy AWS, vẽ architecture, seller registration, category management và preview tài liệu trước khi mua. <br>- Chuẩn bị định hướng cho giai đoạn phát triển tiếp theo. | 19/06/2026 | 19/06/2026 | |


### Kết quả đạt được tuần 9:

* Hoàn thiện prototype có đăng ký, đăng nhập, tìm kiếm sản phẩm và các chức năng marketplace cơ bản.
* Xây dựng chức năng quản lý người dùng phía admin gồm ban và unban user.
* Hỗ trợ sản phẩm số như tài liệu PDF/Word và file mô hình 3D.
* Hiển thị sản phẩm dạng 3D model trực tiếp trên web bằng 3D viewer.
* Tích hợp xác nhận thanh toán theo thời gian thực thông qua SePay API và xử lý thông báo giao dịch.
* Xác định các việc còn tồn đọng: vẽ architecture chính thức trên AWS, thử deploy AWS, đăng ký seller, quản lý danh mục và xem trước tài liệu trước khi mua.
