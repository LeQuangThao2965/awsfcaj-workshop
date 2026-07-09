---
title: "Triển khai frontend và lưu trữ sản phẩm trên S3"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Frontend ban đầu dự kiến deploy bằng S3 + CloudFront. Vì CloudFront bị chặn xác minh tài khoản, bản demo thực tế dùng **Vercel** cho frontend và dùng **S3** đúng vai trò lưu trữ product assets. Vì vậy sơ đồ kiến trúc ghi **"Vercel hoặc CloudFront"** ở phần frontend delivery.

Phần này cũng trình bày cải tiến storage quan trọng nhất của workshop: chuyển file sản phẩm từ lưu trữ local trên EC2 sang **Amazon S3**. Trước khi chuyển đổi, file upload nằm trong `backend/storage/products` của EC2. Sau khi chuyển đổi, product mới được upload vào S3 trong prefix `products/`.

#### Lý do dùng S3

- S3 tách storage của sản phẩm khỏi compute/backend.
- S3 tránh mất file khi git pull, thay EC2 instance hoặc thay đổi local disk.
- S3 phù hợp để mở rộng lưu trữ document, image, thumbnail và 3D model.
- File vẫn private vì bật S3 Block Public Access và người dùng truy cập thông qua backend.

#### Nội dung

- [Deploy frontend lên Vercel](5.4.1-deploy-frontend-vercel/)
- [API rewrite và webhook forwarding](5.4.2-api-rewrite-webhook-forwarding/)
- [IAM Role và migration code backend sang S3](5.4.3-iam-role-s3-migration/)
- [Kiểm thử upload, preview và download qua S3](5.4.4-test-s3-upload-preview-download/)
