---
title: "Tổng quan Workshop"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Mục tiêu dự án

**DaiMarket** là nền tảng marketplace dành cho sản phẩm số như tài liệu PDF/Word và mô hình 3D. Hệ thống cho phép người dùng xem danh sách sản phẩm, xem chi tiết, xem trước mô hình 3D hoặc tài liệu mẫu, mua sản phẩm và truy cập sản phẩm đã mua trong thư viện cá nhân.

<img alt="hosted zone" src="../../../images/5-Workshop/5.1-workshop-overview/project_architecture.png">

#### Kiến trúc đã triển khai

| Tầng | Dịch vụ / công nghệ | Vai trò trong hệ thống |
| --- | --- | --- |
| Frontend | Vercel / React + Vite | Phục vụ giao diện web, SPA routing và bản build frontend. |
| API | Amazon EC2 / Node.js, Express, PM2 | Chạy các route backend cho auth, products, admin, orders, webhook và library. |
| Database | Amazon RDS / PostgreSQL | Lưu users, roles, categories, products, orders, order items, payment methods, seller data và trạng thái mua hàng. |
| Storage | Amazon S3 | Lưu tài sản sản phẩm như thumbnail, document, file gốc và 3D preview model. |
| Security | IAM Role | Cho phép EC2 PutObject, GetObject, DeleteObject và ListBucket trong phạm vi prefix S3 đã cấu hình. |
| Payment | SePay webhook | Nhận thông báo chuyển khoản và cập nhật đơn hàng từ PENDING sang SUCCESS. |

#### Ghi chú kiến trúc

- Kiến trúc mục tiêu ban đầu có **CloudFront, Route 53, WAF** và có thể dùng **Secrets Manager**. Trong bản demo thực tế, **Vercel thay CloudFront** vì tài khoản AWS chưa được xác minh để tạo CloudFront distribution.
- Backend vẫn chạy trên AWS EC2 và sử dụng **RDS + S3 + IAM Role** — đây là các dịch vụ AWS quan trọng nhất của phần triển khai hiện tại.
- **S3 Block Public Access** luôn được bật. File sản phẩm không public; backend kiểm tra quyền sở hữu trước khi stream file về trình duyệt.
- Hiện tại backend stream file từ S3. Cải tiến production sau này là tạo **presigned URL** ngắn hạn sau khi đã kiểm tra quyền sở hữu.

<!-- INSERT FIGURE 5.1: Sơ đồ kiến trúc triển khai sử dụng Vercel hoặc CloudFront cho frontend, EC2 backend, RDS PostgreSQL, S3 private product storage, IAM Role và SePay webhook. -->
