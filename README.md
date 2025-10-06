# 🏥 Healthcare Revenue Analysis using SQL

This SQL project explores a healthcare database to analyze doctors’ total revenue, treatment patterns, and appointment status distribution.  
It demonstrates proficiency in joins, aggregations, CTEs (Common Table Expressions), and window functions to extract actionable insights from multiple related tables.

## 🗂️ Project Overview
The project focuses on:
- Combining data from multiple tables (`appointments`, `treatments`, `doctors`, `billing`, and `patients`).
- Calculating **total revenue per doctor** and identifying their **top 3 treatment types** by revenue.
- Analyzing appointment status distribution (e.g., Completed, Cancelled, Pending).

## 🧰 Tools & Skills
- SQL Server / SSMS 
- Joins (INNER JOIN)  
- Aggregate Functions (SUM, COUNT)  
- GROUP BY and ORDER BY
- Common Table Expressions (CTEs)  
- Window Functions (ROW_NUMBER)  

## 🧠 Key SQL Concepts Demonstrated
```sql
-- Count of appointments by status
SELECT status, COUNT(*) AS Count
FROM appointments
GROUP BY status
ORDER BY Count DESC;

 Top 3 treatment types by total revenue for each doctor
WITH doc_tr AS (
    SELECT d.doctor_id,
           d.last_name,
           d.first_name,
           t.treatment_type,
           SUM(t.cost) AS total_rev
    FROM treatments t
    JOIN appointments a ON t.appointment_id = a.appointment_id
    JOIN doctors d ON a.doctor_id = d.doctor_id
    GROUP BY d.doctor_id, d.last_name, d.first_name, t.treatment_type
)
SELECT doctor_id, last_name, first_name, treatment_type, total_rev
FROM (
    SELECT doc_tr.*,
           ROW_NUMBER() OVER (PARTITION BY doctor_id ORDER BY total_rev DESC) AS rn
    FROM doc_tr
) x
WHERE rn <= 3
ORDER BY doctor_id, total_rev DESC;
````

### 📊 Insights

Displays appointment distribution by status to understand patient engagement.

Calculates each doctor’s top 3 most profitable treatment types.

Provides a clear, query-based approach for healthcare revenue optimization. 

## 👤 Author

Ifeoluwa Boriowo
📧 boriowoifeoluwa@gmail.com
