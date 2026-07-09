---
title: "Dọn dẹp, kiểm soát chi phí và hướng phát triển"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Quyết định thiết kế tiết kiệm chi phí

- Không dùng NAT Gateway, ALB, Multi-AZ RDS, WAF trả phí hay RDS Proxy — bản demo đã được chạy với footprint nhỏ nhất có thể.
- Bật AWS Budget alerts để phát hiện chi phí bất thường.
- CloudFront, Route 53 và ACM có thể dùng, nhưng phải đảm bảo kiểm soát tốt chi phí & nên cập nhật Budget alerts cho mỗi dịch vụ được dùng.

#### Dọn dẹp

- Stop hoặc terminate EC2 instance.
    + Bước 1: Vào AWS Console -> Search dịch vụ EC2 -> Chọn Instance muốn xóa -> Click vào Instance state -> Terminate (delete) Instance 
    ![alt text](../../../images/5-Workshop/5.6-cleanup-cost-next-steps/1st-step-click-Instance-state.png)
    + Bước 2: Ấn delete và xác nhận delete 
    ![alt text](../../../images/5-Workshop/5.6-cleanup-cost-next-steps/2nd-step-terminate-instance-ec2.png)

- Xóa RDS instance (snapshot lần cuối nếu cần giữ dữ liệu).
![alt text](../../../images/5-Workshop/5.6-cleanup-cost-next-steps/rds-delete-databases-save-snapshot-if-needed.png)

- Empty và xóa S3 bucket, hoặc chỉ giữ các object `products/` cần thiết.
![alt text](../../../images/5-Workshop/5.6-cleanup-cost-next-steps/first-step-s3-delete.png)
![alt text](../../../images/5-Workshop/5.6-cleanup-cost-next-steps/second-step-s3-delete-empty-bucket-before-deletion.png)

- Gỡ IAM role và inline policy.
![alt text](../../../images/5-Workshop/5.6-cleanup-cost-next-steps/delete-iam-role.png)

- Xóa project Vercel hoặc tạm dừng deployment, do Vercel là miễn phí nên không cần xóa.

#### Hướng phát triển tiếp theo

- HTTPS cho backend qua custom domain với proxy, ALB hoặc API Gateway.
- Presigned URL để download trực tiếp từ S3 có giới hạn thời gian.
- Chuyển secrets sang SSM Parameter Store hoặc Secrets Manager.
- CloudWatch logs, metrics và alarms cho backend.
- Soft delete cho sản phẩm đã có trong đơn hàng.
- Pipeline CI/CD cho backend và frontend.
