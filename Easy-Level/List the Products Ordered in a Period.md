# Products Ordered in February 2020

This problem requires finding the names of products that have at least 100 units ordered in February 2020, along with the total quantity ordered.

## Table Schema

### Table: `Products`
| Column Name      | Type    |
|------------------|---------|
| product_id       | int     |
| product_name     | varchar |
| product_category | varchar |
- `product_id` is the primary key (column with unique values) for this table.
- This table contains data about the company's products.

### Table: `Orders`
| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| order_date    | date    |
| unit          | int     |
- This table may have duplicate rows.
- `product_id` is a foreign key (reference column) to the `Products` table.
- `unit` is the number of products ordered on `order_date`.

## Problem Statement
Write a solution to get the names of products that have at least 100 units ordered in February 2020, along with the amount ordered for each product.

### Example Input

#### `Products` Table:
| product_id  | product_name          | product_category |
|-------------|-----------------------|------------------|
| 1           | Leetcode Solutions    | Book             |
| 2           | Jewels of Stringology | Book             |
| 3           | HP                    | Laptop           |
| 4           | Lenovo                | Laptop           |
| 5           | Leetcode Kit          | T-shirt          |

#### `Orders` Table:
| product_id   | order_date   | unit     |
|--------------|--------------|----------|
| 1            | 2020-02-05   | 60       |
| 1            | 2020-02-10   | 70       |
| 2            | 2020-01-18   | 30       |
| 2            | 2020-02-11   | 80       |
| 3            | 2020-02-17   | 2        |
| 3            | 2020-02-24   | 3        |
| 4            | 2020-03-01   | 20       |
| 4            | 2020-03-04   | 30       |
| 4            | 2020-03-04   | 60       |
| 5            | 2020-02-25   | 50       |
| 5            | 2020-02-27   | 50       |
| 5            | 2020-03-01   | 50       |

### Expected Output
| product_name       | unit    |
|--------------------|---------|
| Leetcode Solutions | 130     |
| Leetcode Kit       | 100     |

### Explanation:
- Products with `product_id = 1` were ordered in February for a total of (60 + 70) = 130 units.
- Products with `product_id = 2` were ordered in February for a total of 80 units (which is below the threshold).
- Products with `product_id = 3` were ordered in February for a total of (2 + 3) = 5 units.
- Products with `product_id = 4` were not ordered in February 2020.
- Products with `product_id = 5` were ordered in February for a total of (50 + 50) = 100 units.

## SQL Query Solution
The following SQL query will give the desired result:

```sql
SELECT 
    Products.product_name, 
    SUM(orders.unit) AS unit 
FROM orders 
JOIN products ON products.product_id = orders.product_id 
WHERE orders.order_date BETWEEN '2020-02-01' AND '2020-02-29' 
GROUP BY products.product_id 
HAVING SUM(orders.unit) >= 100;
