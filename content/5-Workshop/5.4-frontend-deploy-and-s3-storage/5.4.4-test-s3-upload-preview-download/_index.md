---
title: "Testing S3 upload, preview, and download"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4 </b> "
---

#### Backend API tests


```bash
curl http://localhost:5000/health
curl http://localhost:5000/api/products
curl -I http://localhost:5000/api/products/<product-id>/thumbnail
# Expected: 200 with Content-Type image/png or image/jpeg
curl -I http://localhost:5000/api/products/<product-id>/preview
curl -I http://localhost:5000/api/products/<product-id>/model
```

The displayed content should be "HTTP/1.1 200 OK"
```bash
ubuntu@ec2-backend:~/backend$ curl -I localhost:5000/api/products
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

ubuntu@ec2-backend:~/backend$ curl -I localhost:5000/api/products/<product-id>/thumbnail
HTTP/1.1 200 OK
Content-Type: image/png
```
#### Web flow tests

- Admin uploads a new document product and a new 3D product — files land in S3 under `products/`.
- Buyer flow: purchase, search, order history, and library all work end to end.
- Download is only allowed after the ownership check passes.
- The Order History download button reuses the library download service — there is no separate `/api/products/:id/download` endpoint.

#### Verify objects in S3

```bash
aws s3 ls s3://marketplace-frontend-thao/products/ --region us-east-1 | tail -20
```

<!-- INSERT FIGURE 5.9: Ảnh S3 console liệt kê object mới trong products/ sau khi upload từ web app. -->
### Demo images of the products inside the S3 bucket.
![alt text](../../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/S3-products-after-upload-to-S3.png)
<!-- INSERT FIGURE 5.10: Ảnh trình duyệt hiển thị thumbnail sản phẩm, preview 3D và download thành công từ library. -->
### Frontend demo
![alt text](../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/Web-preview-all-product-with-thumbnail.png)
![alt text](../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/Web-preview-3d-product.png)
![alt text](../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.4-test-s3-upload-preview-download/Web-preview-for-pdf-file.png)