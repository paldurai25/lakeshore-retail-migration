# Lakeshore Retail – sample data

Lakeshore Retail is a fictional retailer with 12 stores in Michigan and nearby states, plus online sales. This data is used throughout the learning project (Aug–Oct 2026 sales).

| File | Rows | Grain (one row = …) |
|---|---|---|
| `stores.csv` | 12 | one store |
| `products.csv` | 40 | one product |
| `customers.csv` | 602 | one customer (with deliberate duplicates) |
| `customers_changes_2026-11-02.csv` | 25 | a changed customer record (city, state or loyalty tier) |
| `orders.csv` | 6,000 | one order (header) |
| `order_lines.csv` | 15,019 | one product line within an order |

## How the files relate

- `orders.customer_id` → `customers.customer_id` (blank = guest checkout)
- `orders.store_id` → `stores.store_id` (`ONLINE` = web order, no physical store)
- `order_lines.order_id` → `orders.order_id`
- `order_lines.product_id` → `products.product_id`

## Business rules

- Line revenue = `quantity × unit_price × (1 − discount_pct)`
- Line cost = `quantity × products.unit_cost`
- Only `COMPLETED` orders count as sales. `RETURNED` and `CANCELLED` are reported separately.

## Data quality issues (planted on purpose – find and handle them)

This data contains realistic problems. Finding them is part of the exercise; try before reading the list.

<details>
<summary>Spoiler: the planted issues</summary>

1. Exact duplicate rows in `customers.csv`
2. Missing emails for some customers
3. Inconsistent `state` casing (`mi` vs `MI`)
4. Guest orders with a blank `customer_id`
5. Order lines pointing to a product that doesn't exist (`P9999`)
6. Order lines with a negative quantity
7. A discontinued product (`is_active = N`) that still appears in orders
8. `customers_changes_2026-11-02.csv` holds address and tier changes, used later for slowly changing dimension (SCD) Type 2

</details>
