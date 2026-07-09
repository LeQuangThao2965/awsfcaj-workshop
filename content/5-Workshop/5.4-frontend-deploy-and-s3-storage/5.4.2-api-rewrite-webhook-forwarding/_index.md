---
title: "API rewrite and webhook forwarding"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

#### API routing

The frontend must call the API with relative paths such as `/api/products` and `/api/auth/login`. If the production bundle still calls `localhost:3000` or `localhost:5000`, the browser resolves those to the visitor's own machine, causing a **Network Error**. The production bundle was fixed to use relative `/api/...` paths only.

#### SePay webhook forwarding

The old ngrok link was replaced with the deployed HTTPS domain. Vercel forwards webhook requests to the EC2 backend.

```text
SePay webhook URL will look like this if the setup was correct:
https://daiai-aws.vercel.app/webhook/sepay

Backend route:
POST /webhook/sepay
Authorization: Apikey <SEPAY_API_KEY>
```

#### Verification

For ease of testing, the following command can be used to test products status if the backend has been successfully set up in EC2.
```bash
curl https://daiai-aws.vercel.app/api/products
curl http://localhost:5000/api/products
# Browser checks:
# - Register/Login
# - Product listing
# - Admin routes include the Authorization header
```
