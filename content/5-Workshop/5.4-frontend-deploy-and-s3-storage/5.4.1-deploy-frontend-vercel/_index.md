---
title: "Deploy frontend to Vercel"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

#### Vercel settings

| Setting | Value |
| --- | --- |
| Framework preset | Vite |
| Root directory | `frontend` |
| Install command | `npm install` |
| Build command | `npm run build` |
| Output directory | `dist` |
| Production domain | `https://daiai-aws.vercel.app` |

![alt text](<../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.1-deploy-frontend-vercel/Create vercel.png>)

#### SPA routing fix

Direct navigation to routes such as `/login` or `/products` can fail unless Vercel rewrites SPA routes to `index.html`. This was fixed during deployment with the following `vercel.json` rewrites:

```json
{
  "rewrites": [
    { "source": "/api/:path*", "destination": "http://<EC2-PUBLIC-IP>:5000/api/:path*" },
    { "source": "/webhook/:path*", "destination": "http://<EC2-PUBLIC-IP>:5000/webhook/:path*" },
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

<!-- INSERT FIGURE 5.7: Screenshot of the Vercel project settings/deployment and a browser test of direct /login route access. -->
