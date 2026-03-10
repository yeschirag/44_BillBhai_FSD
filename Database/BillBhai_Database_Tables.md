# Database Tables Documentation

This document provides example records for each table in the BillBhai Retail Order Processing and Billing System to illustrate how data is stored.

---

# 1. Customer Management Module

## CUSTOMER

Purpose: Stores customer identity and contact information.

| customer_id | name | mobile_no | email | address |
|---|---|---|---|---|
| 1 | Arun Kumar | 9876543210 | arun@gmail.com | 12, MG Road, Chennai |
| 2 | Sneha Rao | 9123456789 | sneha@gmail.com | 45, Anna Nagar, Chennai |

---

## LOYALTY_MEMBER

Purpose: Tracks reward points and tier level earned by customers through purchases.

| member_id | customer_id | points_balance | tier_level | joined_date |
|---|---|---|---|---|
| 1 | 1 | 350 | Silver | 2024-01-15 |
| 2 | 2 | 80 | Bronze | 2024-03-01 |

---

# 2. Staff Management Module

## STAFF

Purpose: Stores all staff members including cashiers, return handlers, inventory managers, delivery managers, and admins.

| staff_id | name | role | email | mobile_no |
|---|---|---|---|---|
| 1 | Ravi Shankar | Cashier | ravi@billbhai.com | 9001122334 |
| 2 | Meena Pillai | Return_Handler | meena@billbhai.com | 9002233445 |
| 3 | Karthik Das | Inventory_Manager | karthik@billbhai.com | 9003344556 |
| 4 | Divya Menon | Delivery_Manager | divya@billbhai.com | 9004455667 |
| 5 | Admin User | Admin | admin@billbhai.com | 9005566778 |
| 6 | Self Checkout | Self_Checkout | NULL | NULL |

---

# 3. Product & Supplier Module

## SUPPLIER

Purpose: Stores supplier details who provide products to the store.

| supplier_id | name | mobile_no | email | address | gst_no |
|---|---|---|---|---|---|
| 1 | Textile Hub Pvt Ltd | 9111222333 | supply@textilehub.com | 10, Industrial Area, Coimbatore | 33AABCT1234F1Z5 |
| 2 | Fashion World Co | 9222333444 | info@fashionworld.com | 5, Export Zone, Tirupur | 33AABCF5678G1Z2 |

---

## PRODUCT

Purpose: Stores all sellable items offered by the store.

| product_id | supplier_id | name | barcode | category | price | size | description |
|---|---|---|---|---|---|---|---|
| 1 | 1 | Blue Denim Jeans | 8901234567890 | Men's Clothing | 1299.00 | 32 | Regular fit blue jeans |
| 2 | 1 | White Cotton Shirt | 8901234567891 | Men's Clothing | 799.00 | L | Formal white shirt |
| 3 | 2 | Floral Kurti | 8901234567892 | Women's Clothing | 999.00 | M | Cotton printed kurti |

---

## INVENTORY

Purpose: Tracks stock availability for each product.

| inventory_id | product_id | stock_available | reorder_level | location | last_updated |
|---|---|---|---|---|---|
| 1 | 1 | 45 | 10 | Rack A1 | 2024-03-01 09:00:00 |
| 2 | 2 | 8 | 10 | Rack A2 | 2024-03-01 09:00:00 |
| 3 | 3 | 62 | 15 | Rack B1 | 2024-03-01 09:00:00 |

---

# 4. Order & Billing Module

## ORDER

Purpose: Stores customer orders from creation to completion.

| order_id | customer_id | staff_id | order_date | order_type | total_amount* | checkout_mode | status | discount_amount |
|---|---|---|---|---|---|---|---|---|
| 1 | 1 | 1 | 2024-03-01 10:15:00 | in-store | 2098.00 | cashier | completed | 0.00 |
| 2 | 2 | 6 | 2024-03-01 11:30:00 | in-store | 999.00 | self_checkout | completed | 100.00 |
| 3 | 1 | 1 | 2024-03-02 14:00:00 | delivery | 1299.00 | cashier | pending | 0.00 |

> *total_amount is a derived attribute — calculated from ORDER_ITEMs, not stored in DB.

---

## ORDER_ITEM

Purpose: Stores individual product entries within each order.

| order_item_id | order_id | product_id | quantity | item_price | subtotal* |
|---|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 1299.00 | 1299.00 |
| 2 | 1 | 2 | 1 | 799.00 | 799.00 |
| 3 | 2 | 3 | 1 | 999.00 | 999.00 |
| 4 | 3 | 1 | 1 | 1299.00 | 1299.00 |

> *subtotal is a derived attribute — calculated as quantity × item_price, not stored in DB.

---

## BILL

Purpose: Financial document generated after order confirmation showing payable amount.

| bill_no | order_id | bill_date | subtotal* | tax_amount | total_amount* | discount_amount |
|---|---|---|---|---|---|---|
| 1 | 1 | 2024-03-01 10:16:00 | 2098.00 | 251.76 | 2349.76 | 0.00 |
| 2 | 2 | 2024-03-01 11:31:00 | 999.00 | 119.88 | 1018.88 | 100.00 |

> *subtotal and total_amount are derived attributes — not stored in DB.

---

## PAYMENT

Purpose: Records payment made against a bill.

| payment_id | bill_no | payment_date | payment_method | payment_status | amount_paid |
|---|---|---|---|---|---|
| 1 | 1 | 2024-03-01 10:17:00 | UPI | success | 2349.76 |
| 2 | 2 | 2024-03-01 11:32:00 | card | success | 1018.88 |

---

## INVOICE

Purpose: Finalized billing document issued after payment for accounting and GST purposes.

| invoice_no | bill_no | invoice_date | invoice_status |
|---|---|---|---|
| 1 | 1 | 2024-03-01 10:18:00 | issued |
| 2 | 2 | 2024-03-01 11:33:00 | issued |

---

# 5. Delivery Module

## DELIVERY

Purpose: Tracks delivery lifecycle for orders marked for home delivery.

| delivery_id | order_id | partner_name | dispatch_date | delivery_date | status |
|---|---|---|---|---|---|
| 1 | 3 | Bluedart | 2024-03-02 16:00:00 | 2024-03-04 12:00:00 | delivered |

---

# 6. Returns Module

## RETURN

Purpose: Records product returns, refunds, and exchanges by customers.

| return_id | order_id | staff_id | return_date | reason | refund_amount | status | return_type |
|---|---|---|---|---|---|---|---|
| 1 | 1 | 2 | 2024-03-05 10:00:00 | Defective item | 1299.00 | approved | refund |
| 2 | 2 | 2 | 2024-03-06 11:00:00 | Wrong size | 999.00 | approved | exchange |

---

# 7. Company Module

## COMPANY

Purpose: Stores the retail store's own company details for invoicing and GST purposes.

| company_id | company_name | address | email | phone_no | gst_no |
|---|---|---|---|---|---|
| 1 | BillBhai Retail Pvt Ltd | 1, Commerce Street, Chennai | contact@billbhai.com | 9900000001 | 33AABCB9999H1Z1 |

---

# 8. Reporting Module

## REPORT

Purpose: Stores generated operational and sales reports.

| report_id | report_type | generated_by | generated_at | period_from | period_to |
|---|---|---|---|---|---|
| 1 | sales | 5 | 2024-03-01 20:00:00 | 2024-03-01 | 2024-03-01 |
| 2 | inventory | 3 | 2024-03-01 20:05:00 | 2024-03-01 | 2024-03-01 |
| 3 | returns | 5 | 2024-03-07 09:00:00 | 2024-03-01 | 2024-03-07 |
