# Taiwo-Northwind-Project
---1. Write a query to get Product name and quantity/unit

SELECT product_name, quantity_per_unit
FROM products
----------------------------------------------------------------------
---2. Write a query to get current Product list (Product ID and name). 

SELECT product_id, product_name
FROM products
WHERE discontinued = 0
--------------------------------------------------------------------------------------------
---3. Write a query to get the Products by Category 

SELECT product_name, category_name
FROM products p
JOIN categories c ON p.category_id=C.category_id
-------------------------------------------------------------------------------------
---4. Write a query to get discontinued Product list (Product ID and name)
SELECT product_id, product_name
FROM products
WHERE discontinued = 1
--------------------------------------------------------------------------------------------
---5. Write a query to get most expense and least expensive Product list (name 
and unit price). 

(SELECT product_name, unit_price
FROM products p
ORDER BY unit_price DESC
LIMIT 1)
UNION
(SELECT product_name, unit_price
FROM products p
ORDER BY unit_price ASC
LIMIT 1)
-------------------------------------------------------------------------------------------
---6. Write a query to get Product list (id, name, unit price) where current products 
cost less than $20. 

SELECT product_id, product_name, unit_price
FROM products
WHERE discontinued = 0 AND unit_price < 20
-------------------------------------------------------------------------------------------
---7. Write a query to get Product list (id, name, unit price) where products cost 
between $15 and $25. 

SELECT product_id, product_name, unit_price
FROM products
WHERE unit_price BETWEEN 15 AND 20
------------------------------------------------------------------------------------------

---8.. Write a query to get Product list (name, unit price) of above average price.

SELECT (product_name), unit_price
FROM products
WHERE unit_price > (SELECT AVG(unit_price) FROM products)
ORDER BY unit_price
------------------------------------------------------------------------------------
---9. Write a query to get Product list (name, unit price) of ten most expensive 
products. 

SELECT product_name, unit_price
FROM products
ORDER BY unit_price DESC
LIMIT 10
-------------------------------------------------------------------------------------
---10. Write a query to count current and discontinued products. 

SELECT 
			SUM(CASE WHEN discontinued = 0 THEN 1 ELSE 0 END) AS current_products,
			SUM(CASE WHEN discontinued = 1 THEN 1 ELSE 0 END) AS discontinued_products
FROM products
---------------------------------------------------------------------------------------
---11. . Write a query to get Product list (name, units on order , units in stock) of stock 
is less than the quantity on order.

SELECT product_name, units_on_order, units_in_stock
FROM products
WHERE units_in_stock < units_on_order
------------------------------------------------------------------------------------------------
---12.For each employee, get their sales amount. 

SELECT CONCAT(first_name,' ',last_name) employee_name, ROUND(SUM(od.quantity*od.unit_price*(1-od.discount))) total_sales
FROM employees e
JOIN orders o ON e.employee_id=o.employee_id
JOIN order_details od ON o.order_id=od.order_id
GROUP BY employee_name
ORDER BY total_sales DESC
------------------------------------------------------------------------------------------------------
---14. . For each category, get the list of products sold and the total sales amount per 
product 

SELECT category_name, product_name products_sold, ROUND(SUM(od.quantity*od.unit_price*(1-od.discount))) total_sales_amount
FROM categories c
JOIN products p ON c.category_id=p.category_id
JOIN order_details od ON p.product_id=od.product_id
GROUP BY category_name, products_sold
ORDER BY total_sales_amount DESC
---------------------------------------------------------------------------------------------------------
---15. Write a query that shows sales figures by categories for the year 1997 alone. 

SELECT category_name, ROUND(SUM(od.quantity*od.unit_price*(1-od.discount))) aales_figure                     
FROM categories c
JOIN products p ON c.category_id=p.category_id
JOIN order_details od ON p.product_id=od.product_id
JOIN orders o ON od.order_id=o.order_id
WHERE EXTRACT(YEAR FROM o.order_date) = 1997
GROUP BY  category_name
