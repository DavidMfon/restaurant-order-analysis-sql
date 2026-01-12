# Restaurant Order Analysis in SQL

### Project Overview
---

This project analyzes customer ordering behavior at Taste of the World Café, a restaurant known for its diverse menu and generous portions.

At the beginning of the year, the restaurant introduced a new menu, and as a newly hired Data Analyst, the goal of this project is to explore customer transaction data to understand which menu items are performing well, which are underperforming, and what customer preferences reveal about spending behavior.
The analysis focuses on exploring the menu, examining order patterns, and combining both datasets to uncover actionable business insights.

### Data Sources
---

Restaurant Orders Dataset

The dataset contains two tables:
`menu_items`
`order_details`
These tables include menu information such as item names, categories, and prices, as well as customer order transactions.
[Click here to access the dataset](https://drive.google.com/drive/folders/1-9vxtwceLcslbfu7xfRjMiV0fbeBbJy8) 

### Tools
---

**SQL:** Used for data exploration and analysis

### Data Cleaning
---

No data cleaning was required for this project.
The dataset was already well structured, consistent, and ready for analysis.

### Exploratory Data Analysis

**Objective 1: Explore the Menu Items Table**

*The goal was to understand the structure and pricing of the new menu.
Key explorations included:*

1. Viewing all menu items

```sql
SELECT * FROM menu_items;
```

2. Counting the total number of items on the menu

```sql
SELECT COUNT(*) FROM menu_items;
```

3. Identifying the least and most expensive items

```sql
SELECT * FROM menu_items
ORDER BY price DESC;
```

4. Analyzing Italian dishes on the menu

```sql
SELECT * COUNT(*) FROM menu_items
WHERE category REGEXP 'Italian';
```

5. Counting dishes per category

```sql
SELECT category, COUNT(item_name)
FROM menu_items
GROUP BY category;
```

6. Calculating average dish prices by category

```sql
SELECT category, AVG(price) AS avg_dish_price
FROM menu_items
GROUP BY category;
```

**Objective 2: Explore the Order Details Table**

This step focused on understanding customer ordering behavior and transaction volume.
Key questions answered:

1. What is the date range of the orders?

```sql
SELECT MIN(order_date), MAX(order_date)
FROM order_details;
```

2. How many orders were placed within this period?

```sql
SELECT COUNT(DISTINCT order_id)
FROM order_details;
```

3. How many orders were made within the date range?

```sql
SELECT COUNT (DISTINCT order_id) FROM order_details;
```
4. How many items were ordered within this date range? 

```sql
SELECT COUNT(*) FROM order_details;
```

5. Which orders had the most number of items? 

```sql
SELECT order_id, COUNT(item_id) 
FROM order_details
GROUP BY order_id
ORDER BY COUNT(item_id) DESC;
```

6. How many orders had more than 12 items

```sql
WITH CTE AS (SELECT order_id, COUNT(item_id) 
FROM order_details
GROUP BY order_id
HAVING COUNT(item_id) > 12) 
SELECT COUNT(*) FROM CTE;
```
OR

```sql
SELECT COUNT (*) FROM
(SELECT order_id, COUNT(item_id) AS num_items
FROM order_details
GROUP BY order_id
HAVING num_items > 12) AS num_orders
```
**NOTE: The goal here was to explore the order_details table**

Data Analysis
Objective 3: Analyze Customer Behavior
To gain deeper insights, the menu_items and order_details tables were combined.
Key analyses performed:
1. Combine the menu_items and order details tables into a single table. 
SELECT *
FROM order_details AS o
LEFT JOIN menu_items AS m
           ON o. item_id = m.menu_item_id;
2. What were the least and most ordered items? What categories were they in? 
SELECT item_name, COUNT(order_details_id) AS num_purchases
FROM order_details AS o
LEFT JOIN menu_items AS m
         ON o.item_id = m.menu_item_id
GROUP BY item_name
ORDER BY num_purchase; 
3. What were the top 5 orders that spent the most money? 
SELECT order_id, SUM(price) AS total_spend
FROM order_details AS o
LEFT JOIN menu_items AS m
          ON o.item_id = m.menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;
4.  View the details of the highest spend order. What insights can you gather from here 
SELECT category, COUNT(item_id) AS num_items
FROM order_details AS o
LEFT JOIN menu_items AS m
          ON o.item_id = m.menu_item_id
WHERE order_id = 440
GROUP BY category;
Note: The insights gathered here is that we should keep this expensive Italian food in order_menu because people seem to order them a lot. 
5. View the details of the top 5 highest spend orders. What insight can you gather from the top 5 highest spend orders. 
SELECT order_id, category, COUNT(item_id) AS num_items
FROM order_details AS o
LEFT JOIN menu_items AS m
          ON o.item_id = m.menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675) 
GROUP BY order_id, category;

