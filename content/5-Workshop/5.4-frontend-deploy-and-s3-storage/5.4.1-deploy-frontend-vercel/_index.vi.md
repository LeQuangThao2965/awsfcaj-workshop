---
title: "Deploy frontend lên Vercel"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

#### Thông số Vercel

| Thiết lập | Giá trị |
| --- | --- |
| Framework preset | Vite |
| Root directory | `frontend` |
| Install command | `npm install` |
| Build command | `npm run build` |
| Output directory | `dist` |
| Production domain | `https://daiai-aws.vercel.app` |

![alt text](<../../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.1-deploy-frontend-vercel/Create vercel.png>)

#### Fix SPA routing

Truy cập trực tiếp các route như `/login` hoặc `/products` sẽ lỗi nếu chưa rewrite SPA về `index.html`. Lỗi này đã được fix trong quá trình deploy bằng cấu hình `vercel.json` sau:

```json
{
  "rewrites": [
    { "source": "/api/:path*", "destination": "http://<EC2-PUBLIC-IP>:5000/api/:path*" },
    { "source": "/webhook/:path*", "destination": "http://<EC2-PUBLIC-IP>:5000/webhook/:path*" },
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

<!-- INSERT FIGURE 5.7: Ảnh Vercel deployment/settings và test truy cập trực tiếp /login thành công. -->
