---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Ổn định cấu trúc dự án sau giai đoạn triển khai ban đầu.
* Refactor cấu trúc frontend và backend để dễ bảo trì hơn.
* Tiếp tục hoàn thiện database schema và data structure.
* Sửa lỗi phát sinh và chuẩn bị cho giai đoạn phát triển tính năng cốt lõi.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Rà soát cấu trúc source code hiện tại và xác định các phần khó bảo trì. <br>- Lập kế hoạch cải thiện cách phân tách component frontend và module backend. <br>- Định nghĩa quy ước đặt tên cho file, route và service. | 25/05/2026 | 25/05/2026 | |
| 3 | - Refactor cấu trúc backend. <br>- Tách rõ route definition, controller logic, service logic và model access. <br>- Kiểm tra format response API để đồng nhất. | 26/05/2026 | 26/05/2026 | |
| 4 | - Refactor cấu trúc frontend. <br>- Sắp xếp React pages, reusable components, API service files và logic trạng thái. <br>- Loại bỏ code trùng lặp hoặc không còn sử dụng nếu có. | 27/05/2026 | 27/05/2026 | |
| 5 | - Rà soát và cập nhật thiết kế database/data model. <br>- Bổ sung field và ràng buộc cho users, products, product files, categories, purchases và transactions. <br>- Kiểm tra quan hệ giữa các entity trước khi tiếp tục phát triển. | 28/05/2026 | 28/05/2026 | |
| 6 | - Kiểm thử các chức năng hiện có sau khi refactor. <br>- Sửa lỗi phát sinh do thay đổi cấu trúc. <br>- Chuẩn bị dự án cho các chức năng quản lý sản phẩm, upload S3 và thanh toán. | 29/05/2026 | 29/05/2026 | |


### Kết quả đạt được tuần 6:

* Tối ưu cấu trúc frontend/backend và phân tách trách nhiệm rõ hơn.
* Sắp xếp lại routes, controllers, services, components và utilities.
* Cập nhật cấu trúc dữ liệu cho người dùng, sản phẩm, file số, danh mục và giao dịch.
* Bổ sung một số trường và ràng buộc để hỗ trợ các chức năng marketplace sau này.
* Kiểm thử cơ bản và sửa lỗi các module hiện có trước khi phát triển tính năng lớn hơn.
