#  Analysis of PizzaSales using SQL
# Retrieve the total number of orders placed.
### SELECT sum(quantity) AS Total_Orders FROM order_details;

# Calculate the total revenue generated from pizza sales.
### SELECT Round(sum(price),2) AS Total_Revenue From pizzas;

# Identify the highest-priced pizza.
### SELECT TOP 1 name,Round(price,2) AS MAX_Price FROM pizza_types
### JOIN pizzas ON pizza_types.pizza_type_id  = pizzas.pizza_type_id
### ORDER BY price desc;

# Identify the most common pizza size ordered.
### SELECT TOP 1 pizza_id,sum(quantity) AS Total_Quantity From order_details
### GROUP BY pizza_id
### ORDER BY sum(quantity) desc;

# List the top 5 most ordered pizza types along with their quantities.
### SELECT TOP 5 PT.pizza_type_id,sum(quantity) AS Total_Quantity From pizza_types PT JOIN pizzas P
### ON PT.pizza_type_id = P.pizza_type_id JOIN
### order_details OD ON OD.pizza_id = P.pizza_id
### GROUP BY PT.pizza_type_id
### ORDER BY sum(quantity) desc;

# Calculate the percentage contribution of each pizza type to total revenue.
### SELECT pt.category,Round(sum(price)/(SELECT ROUND(sum(price),2) AS Total_Price FROM pizzas) *
### 100,2) AS Percentage_Contribution From pizza_types pt JOIN pizzas p
### ON pt.pizza_type_id =p.pizza_type_id
### GROUP BY pt.category;


# Join the necessary tables to find the total quantity of each pizza category ordered.
### SELECT pt.category,sum(od.quantity) AS Total_Quantity FROM pizza_types pt JOIN pizzas p ON
### pt.pizza_type_id = p.pizza_type_id JOIN order_details od ON od.pizza_id = p.pizza_id
### GROUP BY pt.category;

# Determine the distribution of orders by hour of the day.
### SELECT DATEPART(HOUR,OT.order_time) AS Hours,count(OD.quantity) AS Quantity FROM orders OT JOIN order_details OD ON
### OD.order_id = OT.order_id
### GROUP BY DATEPART(HOUR,OT.order_time)
### ORDER BY Hours, Quantity ASC;

# Find the category-wise distribution of pizzas.
### SELECT category,count(name) AS Total FROM pizza_types
### GROUP BY category;

# Group the orders by date and calculate the average number of pizzas ordered per day.
### SELECT date,sum(quantity) AS Total_Quantity 
### FROM orders JOIN order_details ON orders.order_id = order_details.order_id
### GROUP BY date;

# To find average number of pizzzas.
### SELECT AVG(Total_Quantity ) As Avg_Pizzas FROM
### (SELECT date,sum(quantity) AS Total_Quantity 
### FROM orders JOIN order_details ON orders.order_id = order_details.order_id
### GROUP BY date) AS Order_Quantity;

# Determine the top 3 most ordered pizza types based on revenue.
### SELECT top 3 pt.name,sum(p.price) As Revenue FROM pizza_types pt JOIN pizzas p ON
### pt.pizza_type_id =p.pizza_type_id
### GROUP BY pt.name
### ORDER BY sum(p.price) DESC;

# Analyze the cumulative revenue generated over time.
### With CTE AS
### (
### SELECT date AS o_date,sum(price) as rev FROM  orders O JOIN order_details OD
### ON O.order_id = OD.order_id JOIN pizzas p ON OD.pizza_id = p.pizza_id
### GROUP BY date
### )
### SELECT o_date,sum(rev) OVER(order by o_date) as CUM_rev FROM CTE;
