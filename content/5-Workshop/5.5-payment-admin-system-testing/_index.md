---
title: "Payment, admin validation, and system testing"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Grant the admin role

Update the role directly in the database, then log out and log back in so the JWT carries the new role:

```sql
UPDATE "User"
SET "roleId" = (SELECT id FROM "Role" WHERE name = 'admin')
WHERE email = '<admin-email>'
RETURNING id, email, "roleId";
```

#### Category group validation

Product category must match the file type: categories in the **DOCUMENT** group accept document files, categories in the **MODEL_3D** group accept 3D model files.

<!-- INSERT FIGURE 5.11: Screenshot of the admin dashboard showing product approval and category group validation. -->
![alt text](../../images/5-Workshop/5.5-payment-admin-system-testing/add-category.png)
![alt text](../../images/5-Workshop/5.5-payment-admin-system-testing/add-san-pham.png)

#### SePay payment flow

- Webhook route: `POST /webhook/sepay` with header `Authorization: Apikey <SEPAY_API_KEY>`.
- Order code format: `DAIM` + first 8 hex characters of the order UUID, embedded in the transfer description.
- On a matching webhook, the order transitions **PENDING → SUCCESS** and the buyer gains library access through the related `OrderItem` records.

<!-- INSERT FIGURE 5.12: Screenshot of a SePay webhook delivery and the order status changing from PENDING to SUCCESS. -->
![alt text](../../images/5-Workshop/5.5-payment-admin-system-testing/noi-dung-chuyen-khoan.png)
![alt text](../../images/5-Workshop/5.5-payment-admin-system-testing/thanh-toan-thanh-cong.png)

#### Known issue: deleting purchased products

Hard-deleting a product that already appears in orders fails with a foreign key constraint error on `OrderItem`. The recommended fix is **soft delete** (an `isDeleted`/`isActive` flag) so purchase history stays intact while the product disappears from the storefront.
