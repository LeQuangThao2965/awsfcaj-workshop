---
title: "API rewrite và webhook forwarding"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

#### Định tuyến API

Frontend cần gọi API bằng relative path như `/api/products` và `/api/auth/login`. Nếu bundle production còn gọi `localhost:3000` hoặc `localhost:5000` thì trình duyệt sẽ gọi về máy người dùng, gây **Network Error**. Bundle production đã được sửa để chỉ dùng relative path `/api/...`.

#### Forward webhook SePay

Link ngrok cũ được thay bằng domain HTTPS đã deploy. Vercel forward request webhook về backend EC2.

```text
SePay webhook URL đại loại sẽ trông như thế này nếu setup đúng:
https://daiai-aws.vercel.app/webhook/sepay

Backend route:
POST /webhook/sepay
Authorization: Apikey <SEPAY_API_KEY>
```

#### Kiểm tra

Để tiện cho việc kiểm tra, khi sử dụng lệnh dưới đây có thể test product nếu đã setup thành công backend trong EC2
```bash
curl https://daiai-aws.vercel.app/api/products
curl http://localhost:5000/api/products
# Kiểm tra trên browser:
# - Register/Login
# - Product listing
# - Admin routes có Authorization header
```
