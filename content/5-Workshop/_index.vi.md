---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Sàn thương mại sản phẩm số trên Cloud với xem trước 3D — DaiMarket

#### Tổng quan

Phần Workshop này ghi lại quá trình triển khai và kiểm thử thực tế của hệ thống marketplace sản phẩm số trên cloud — **DaiMarket**. Dự án hỗ trợ bán tài liệu PDF/Word và mô hình 3D, có xem trước 3D trên trình duyệt, xác nhận thanh toán qua SePay và lưu trữ tài sản sản phẩm trên Amazon S3.

Workshop tập trung vào việc đưa ứng dụng từ môi trường phát triển lên môi trường cloud tiết kiệm chi phí. Kiến trúc đang chạy thực tế sử dụng **Vercel** cho frontend, **Amazon EC2** cho backend Node.js/Express, **Amazon RDS PostgreSQL** cho dữ liệu quan hệ, **Amazon S3** cho file sản phẩm và **IAM Role** để EC2 truy cập S3 an toàn.

- **Frontend:** React + Vite triển khai trên Vercel. CloudFront là phương án thay thế/mục tiêu tương lai, nhưng chưa dùng trong demo do tài khoản AWS chưa được xác minh để tạo CloudFront.
- **Backend:** Node.js + Express + Prisma 7 chạy trên EC2 bằng PM2.
- **Database:** Amazon RDS PostgreSQL lưu users, roles, products, categories, orders, payment methods và quyền sở hữu trong library.
- **Object storage:** S3 bucket `marketplace-frontend-thao`, prefix `products/` dùng để lưu file sản phẩm riêng tư.
- **Security:** IAM Role `marketplace-ec2-s3-role` gắn cho EC2, cấp quyền tối thiểu với prefix `products/`.
- **Payment:** SePay webhook nhận thông báo giao dịch và cập nhật trạng thái đơn hàng.

<!-- INSERT FIGURE 5.0: Sơ đồ kiến trúc mới có khối 'Vercel hoặc CloudFront', EC2 backend, RDS PostgreSQL, S3 Product Assets, IAM Role và SePay webhook. -->

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview/)
2. [Điều kiện chuẩn bị](5.2-prerequisite/)
3. [Triển khai backend với EC2 và RDS](5.3-backend-deploy-ec2-and-rds/)
4. [Triển khai frontend và lưu trữ sản phẩm trên S3](5.4-frontend-deploy-and-s3-storage/)
5. [Thanh toán, kiểm thử admin và kiểm thử hệ thống](5.5-payment-admin-system-testing/)
6. [Dọn dẹp, kiểm soát chi phí và hướng phát triển](5.6-cleanup-cost-next-steps/)
