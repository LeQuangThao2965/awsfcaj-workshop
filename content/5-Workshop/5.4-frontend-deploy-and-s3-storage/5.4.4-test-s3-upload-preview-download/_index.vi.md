---
title: "Kiểm thử upload, preview và download qua S3"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4 </b> "
---

#### Kiểm thử API backend


```bash
curl http://localhost:5000/health
curl http://localhost:5000/api/products
curl -I http://localhost:5000/api/products/<product-id>/thumbnail
# Mong đợi: 200 với Content-Type image/png hoặc image/jpeg
```

Nội dung hiển thị nên có "HTTP/1.1 200 OK"
```bash
ubuntu@ec2-backend:~/backend$ curl -I localhost:5000/api/products
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

ubuntu@ec2-backend:~/backend$ curl -I localhost:5000/api/products/<product-id>/thumbnail
HTTP/1.1 200 OK
Content-Type: image/png
```
#### Kiểm thử luồng web

- Admin upload một sản phẩm document và một sản phẩm 3D mới — file được lưu vào S3 trong prefix `products/`.
- Luồng buyer: mua hàng, tìm kiếm, lịch sử đơn hàng và thư viện hoạt động đầy đủ.
- Chỉ cho phép download sau khi kiểm tra quyền sở hữu thành công.
- Nút download ở Order History dùng lại service download của library — không có endpoint riêng `/api/products/:id/download`.

#### Kiểm tra object trên S3

```bash
aws s3 ls s3://marketplace-frontend-thao/products/ --region us-east-1 | tail -20
```

<!-- INSERT FIGURE 5.9: Ảnh S3 console liệt kê object mới trong products/ sau khi upload từ web app. -->
### Hình ảnh demo products bên trong S3 bucket
![alt text](../../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/S3-products-after-upload-to-S3.png)
<!-- INSERT FIGURE 5.10: Ảnh trình duyệt hiển thị thumbnail sản phẩm, preview 3D và download thành công từ library. -->
### Hình ảnh demo frontend
![alt text](../../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/Web-preview-all-product-with-thumbnail.png)
![alt text](../../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/Web-preview-3d-product.png)
![alt text](../../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/Web-preview-for-pdf-file.png)