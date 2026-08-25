# 🛍️ Retail Sales SQL Analysis — Primark Case Study

## 📌 Overview

This project was completed as part of my **Level 3 Digital Skills Bootcamp (Data Technician)** with Just IT Training. It uses a Primark-style retail dataset covering inventory, sales, suppliers, and customer transactions to demonstrate core SQL capability: querying, filtering, sorting, grouping, and joining relational data to answer real business questions.

The objective: take raw, disconnected retail data and use SQL to produce insight that a buying, stock, or store operations team can act on directly.

---

## 🗃️ The Dataset

Transaction-level retail data including:

| Column | Description |
|---|---|
| `Transaction_ID`, `Timestamp` | Unique sale reference and date/time |
| `Store_Location` | Store the sale took place in |
| `Product_Category`, `SKU_Code`, `Item_Description` | Product details |
| `Supplier_Name`, `Supplier_Country` | Supplier information |
| `Stock_On_Hand`, `Reorder_Level` | Inventory position |
| `Unit_Cost_GBP`, `Retail_Price_GBP` | Cost and pricing |
| `Units_Sold`, `Total_Sales_GBP`, `Gross_Profit_GBP` | Sales performance |
| `Customer_Age_Group`, `Customer_Gender`, `Payment_Method` | Customer profile |

Alongside the transaction data, I designed a normalised **relational database schema** for a Primark-style product catalogue — covering brands, categories, products, pricing, inventory, reviews, and sellers — as a database design exercise within the same module.

---

## 🧠 SQL Skills Demonstrated

Each skill below is shown against the retail dataset's actual schema, applied to a specific business question rather than as an isolated syntax exercise.

### `SELECT` & `WHERE` — filtering to the question that matters

```sql
-- Sales made in a specific store location
SELECT Transaction_ID, Item_Description, Units_Sold, Total_Sales_GBP
FROM sales_transactions
WHERE Store_Location = 'London (Oxford St)';

-- Stock currently at or below reorder level (needs restocking)
SELECT Item_Description, Stock_On_Hand, Reorder_Level
FROM sales_transactions
WHERE Stock_On_Hand <= Reorder_Level;
```

### `ORDER BY` — surfacing what matters most, first

```sql
-- Highest value transactions first
SELECT Transaction_ID, Item_Description, Total_Sales_GBP
FROM sales_transactions
ORDER BY Total_Sales_GBP DESC;
```

### `GROUP BY` — turning individual rows into a summary

```sql
-- Total sales and profit by product category
SELECT Product_Category,
       SUM(Total_Sales_GBP) AS Total_Sales,
       SUM(Gross_Profit_GBP) AS Total_Profit
FROM sales_transactions
GROUP BY Product_Category
ORDER BY Total_Sales DESC;

-- Average spend by customer age group
SELECT Customer_Age_Group, AVG(Total_Sales_GBP) AS Avg_Spend
FROM sales_transactions
GROUP BY Customer_Age_Group
ORDER BY Avg_Spend DESC;
```

### `JOIN` — connecting related tables

Using the relational schema designed for the wider product catalogue (Brands, Categories, Products, Inventory, Sellers, Product_Sellers), I applied the full range of join types:

```sql
-- Products with their brand and category (INNER JOIN)
SELECT p.Product_Name, b.Brand_Name, c.Category_Name
FROM Products p
INNER JOIN Brands b ON p.Brand_ID = b.Brand_ID
INNER JOIN Categories c ON p.Category_ID = c.Category_ID;

-- All sellers, including products with no seller listed yet (LEFT JOIN)
SELECT s.Seller_Name, p.Product_Name
FROM Sellers s
LEFT JOIN Product_Sellers ps ON s.Seller_ID = ps.Seller_ID
LEFT JOIN Products p ON ps.Product_ID = p.Product_ID;
```

I also worked through **self joins**, **right joins**, **full joins**, and **cross joins**, and can identify the right tool for a given scenario — for example, a self join to compare employees to their managers within the same table, or a right join to retain every record from a reference table regardless of whether a match exists.

---

## 💡 Business Value

This type of analysis delivers clear, actionable insight:

- Identifies which **product categories** drive the most sales and profit — not just the highest transaction volume
- Flags which **stores or SKUs** are at or below reorder level, prioritising restocking decisions
- Reveals which **customer age groups or genders** spend the most, sharpening targeted promotions
- Surfaces which **suppliers** are tied to the highest-margin products, strengthening supplier negotiations
- Highlights **payment method** trends that inform in-store checkout and app investment decisions

---

## 🛠️ Tools Used

- **SQL** (PostgreSQL) — all querying and analysis
- **Excel** — initial dataset review and validation
- Database design principles: primary keys, foreign keys, one-to-many and many-to-many relationships

---

## 📚 Key Takeaway

A query is only as strong as the question behind it. Filtering and sorting get you to individual records quickly, but `GROUP BY` is where raw transactions become genuine insight — and joins are what make it possible to ask questions that span multiple parts of the business at once (products, sellers, and categories together) rather than being confined to a single flat table.

This project also sharpened my judgement on **which join type fits which scenario** — a distinction that's straightforward to explain in theory, but one I've now proven out in practice by building and debugging real multi-table queries.

---

<img width="1030" height="886" alt="Screenshot 2026-08-25 120404" src="https://github.com/user-attachments/assets/5ffeffa3-1acd-4fd2-bd3b-4e7b142cc0e8" />
---
<img width="1048" height="848" alt="Screenshot 2026-08-25 115838" src="https://github.com/user-attachments/assets/cb029f04-c04a-4202-bf1c-527e932139b8" />
