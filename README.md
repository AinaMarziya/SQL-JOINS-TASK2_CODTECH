# 📌 Internship Task – Advanced SQL Analysis (Window Functions, Subqueries, and CTEs)

*Intern Name:* Aina Marziya  
*Internship Organization:* CODTECH  
*Task:* 2 
*Topic:* Advanced SQL Queries for Data Trends and Patterns  
*Tool Used:* MySQL 8.0 Command Line Client

## 🛠 Dataset: Sales Table

```sql
CREATE TABLE Sales (
    sale_id INT,
    employee_id INT,
    sale_amount DECIMAL(10,2),
    sale_date DATE
);

📥 Sample Data:

INSERT INTO Sales VALUES
(1, 101, 5000.00, '2024-01-01'),
(2, 102, 7000.00, '2024-01-02'),
(3, 101, 8000.00, '2024-01-03'),
(4, 103, 6000.00, '2024-01-03'),
(5, 102, 5500.00, '2024-01-04'),
(6, 101, 4000.00, '2024-01-05'),
(7, 103, 9000.00, '2024-01-06'),
(8, 102, 3000.00, '2024-01-07');

🔍 1. WINDOW FUNCTIONS – Finding Running Total

SELECT
  employee_id,
  sale_date,
  sale_amount,
  SUM(sale_amount) OVER (PARTITION BY employee_id ORDER BY sale_date) AS running_total
FROM Sales;

Result:

employee_id	sale_date	sale_amount	running_total

101	2024-01-01	5000.00	5000.00
101	2024-01-03	8000.00	13000.00
101	2024-01-05	4000.00	17000.00
102	2024-01-02	7000.00	7000.00
102	2024-01-04	5500.00	12500.00
102	2024-01-07	3000.00	15500.00
103	2024-01-03	6000.00	6000.00
103	2024-01-06	9000.00	15000.00



🔍 2. SUBQUERY – Top Sale Per Employee

SELECT employee_id, sale_amount
FROM Sales
WHERE (employee_id, sale_amount) IN (
  SELECT employee_id, MAX(sale_amount)
  FROM Sales
  GROUP BY employee_id
);

Result:

employee_id	sale_amount

101	8000.00
102	7000.00
103	9000.00



🔍 3. CTE – Monthly Average Sales per Employee

WITH MonthlySales AS (
  SELECT
    employee_id,
    MONTH(sale_date) AS sale_month,
    AVG(sale_amount) AS avg_sale
  FROM Sales
  GROUP BY employee_id, MONTH(sale_date)
)
SELECT * FROM MonthlySales;

Result:

employee_id	sale_month	avg_sale

101	1	5666.67
102	1	5166.67
103	1	7500.00


📈 Trends/Patterns Observed
Employee 103 has the highest individual sale (₹9000) and average sales.
Running totals help visualize employee sales progress over time.
CTEs make queries cleaner when analyzing grouped data like monthly trends.


✅ Conclusion
This project demonstrates the use of advanced SQL tools such as:
Window Functions (to track trends)
Subqueries (to filter high-performing records)
CTEs (to structure complex queries)


