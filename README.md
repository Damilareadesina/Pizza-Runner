<h1>Damilareadesina - Pizza Runner </h1>

<h2>Description</h2>
  Danny was scrolling through his Instagram feed when something really caught his eye - “80s Retro Styling and Pizza Is The Future!”

Danny was sold on the idea, but he knew that pizza alone was not going to help him get seed funding to expand his new Pizza Empire - so he had one more genius idea to combine with it - he was going to Uberize it - and so Pizza Runner was launched!

Danny started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters (otherwise known as Danny’s house) and also maxed out his credit card to pay freelance developers to build a mobile app to accept orders from customers.
<br />


<h2>Tools Used</h2>

- <b>Structure Query Language </b>



<h2>Environments Used </h2>

- <b>MY SQL WORK BENCH </b>

<h2>Process walk-through:</h2>


  <p align="center"> 
   1. What is the total amount each customer spent at the restaurant?

SELECT sales.customer_id, SUM(menu.price) AS total_spending
FROM sales
LEFT JOIN menu 
	ON menu.product_id = sales.product_id
    GROUP BY sales.customer_id; <br />
	<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<br />
<br />

  <p align="center"> 	
<b> A. Pizza Metrics</b>

 <p align="center">   
1)How many pizzas were ordered?

SELECT COUNT(order_id) AS pizza_order
FROM customer_orders;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
2)How many unique customer orders were made?

SELECT DISTINCT customer_id, COUNT(order_id) AS no_of_orders
FROM customer_orders
GROUP BY customer_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
 3)How many successful orders were delivered by each runner?

SELECT DISTINCT runner_id, COUNT(pickup_time) AS delivered_orders
FROM runner_orders
GROUP BY runner_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
4)How many of each type of pizza was delivered?

SELECT DISTINCT pizza_id, COUNT(runner_orders.order_id) AS order_delivered
FROM runner_orders
JOIN customer_orders
ON customer_orders.order_id = runner_orders.order_id
GROUP BY pizza_id
order by runner_orders.order_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
5)How many Vegetarian and Meatlovers were ordered by each customer?
SELECT   pizza_name, customer_orders.customer_id, COUNT(order_id) AS no_of_orders
FROM customer_orders
JOIN pizza_names
ON customer_orders.pizza_id =pizza_names.pizza_id
GROUP BY customer_id, customer_orders.pizza_id
ORDER BY pizza_name;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
6)What was the maximum number of pizzas delivered in a single order?

SELECT  customer_orders.order_id, COUNT(runner_orders.order_id) AS max_delivery
FROM customer_orders
INNER JOIN runner_orders
ON customer_orders.order_id = runner_orders.order_id
GROUP BY order_id
ORDER BY max_delivery DESC
LIMIT 1;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
7)For each customer, how many delivered pizzas had at least 1 change and how many had no changes?

SELECT DISTINCT  customer_orders.order_id, 
COUNT(runner_orders.order_id) AS orders_with_changes
FROM customer_orders
INNER JOIN runner_orders
ON customer_orders.order_id = runner_orders.order_id
WHERE exclusions or extras REGEXP  '^[0-9]+(,[0-9]+)*$' 
GROUP BY order_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
8)How many pizzas were delivered that had both exclusions and extras?

SELECT DISTINCT  COUNT(customer_orders.order_id), runner_orders.order_id
FROM customer_orders
INNER JOIN runner_orders
ON customer_orders.order_id = runner_orders.order_id
WHERE exclusions REGEXP  '^[0-9]+(,[0-9]+)*$' AND extras REGEXP  '^[0-9]+(,[0-9]+)*$'
;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
9)What was the total volume of pizzas ordered for each hour of the day?
<p align="left"> 
SELECT DAY(order_time) AS day_of_the_month, HOUR(order_time), COUNT(pizza_id) 
FROM customer_orders
GROUP BY order_time
ORDER BY day_of_the_month;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
10)What was the volume of orders for each day of the week?

SELECT WEEK(order_time) AS week ,DAY(order_time) AS day_of_the_month, COUNT(pizza_id) AS volume
FROM customer_orders
GROUP BY DAY(order_time)
ORDER BY day_of_the_month;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
  <p align="center"> 
<b> B. Runner and Customer Experience</b>


 <p align="center">   
1)How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)

SELECT  WEEK(reg_date) AS weeks,
COUNT(runners.runner_id) AS runners_sign_up
FROM runners
GROUP BY WEEK(reg_date); 
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
2)What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order
SELECT runner_id, AVG(TIMEstampdiff(minute,order_time,pickup_time)) AS average_time
FROM runner_orders
INNER JOIN customer_orders
ON runner_orders.order_id = customer_orders.order_id
GROUP BY runner_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
3)Is there any relationship between the number of pizzas and how long the order takes to prepare?
SELECT customer_orders.order_id,
TIMEstampdiff(minute,order_time,pickup_time) AS duration_in_min,
COUNT(customer_orders.order_id) as orders
FROM customer_orders
INNER JOIN runner_orders
ON runner_orders.order_id = customer_orders.order_id
GROUP BY order_id
ORDER BY duration DESC ;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
4)What was the average distance travelled for each customer?

SELECT customer_orders.customer_id, ROUND(AVG(distance),2) 
FROM customer_orders
JOIN runner_orders
ON runner_orders.order_id = customer_orders.order_id
GROUP BY customer_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
5)What was the difference between the longest and shortest delivery times for all orders?

SELECT MAX(duration)-MIN(duration) as time_difference
FROM runner_orders;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
  <p align="center"> 
 6)What was the average speed for each runner for each delivery and do you notice any trend for these values?

SELECT order_id, runner_id, ROUND(AVG(distance/duration * 60),2) AS average_speed
FROM runner_orders
GROUP BY order_id,runner_id
ORDER BY runner_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
7)What is the successful delivery percentage for each runner?
SELECT  runner_id, ROUND(AVG(distance/duration *60) , 0 )AS percetage_delivery
FROM runner_orders
GROUP BY runner_id
ORDER BY runner_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
<b>C. Ingredient Optimisation</b>

CREATE TEMPORARY TABLE pizza_recipe AS(
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',1),',',-1) AS topping_id
FROM pizza_recipes
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',2),',',-1) AS topping_id
FROM pizza_recipes
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',3),',',-1) AS topping_id
FROM pizza_recipes
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',4),',',-1) AS topping_id
FROM pizza_recipes
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',5),',',-1) AS topping_id
FROM pizza_recipes
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',6),',',-1) AS topping_id
FROM pizza_recipes
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',7),',',-1) AS topping_id
FROM pizza_recipes
WHERE LENGTH(toppings) - LENGTH(REPLACE(toppings,',','')) + 1>=7
UNION ALL
SELECT pizza_id,SUBSTRING_INDEX(SUBSTRING_INDEX(toppings,',',8),',',-1) AS topping_id
FROM pizza_recipes
WHERE LENGTH(toppings) - LENGTH(REPLACE(toppings,',','')) + 1>=8
ORDER BY pizza_id);

 <p align="center"> 
1)What are the standard ingredients for each pizza?
<p align="left"> 
SELECT pizza_id, GROUP_CONCAT(topping_name SEPARATOR ',') AS standard_ingredient
FROM pizza_recipe
JOIN pizza_toppings
ON pizza_recipe.topping_id =pizza_toppings.topping_id
GROUP BY pizza_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
2)What was the most commonly added extra?

CREATE TEMPORARY TABLE extra_copy 
AS SELECT
DISTINCT SUBSTRING_INDEX(SUBSTRING_INDEX(extras,',',n),',',-1) AS extra1, order_id
from customer_orders
CROSS JOIN (SELECT 1 AS n 
UNION 
SELECT DISTINCT 2) numbers
WHERE SUBSTRING_INDEX(SUBSTRING_INDEX(extras,',',n),',',-1) REGEXP  '[0-9]' ;
 <img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="left"> 
SELECT extra1, topping_name, count(extra1) AS no_of_extras
FROM extra_copy
INNER JOIN pizza_toppings
ON extra_copy.extra1 = pizza_toppings.topping_id
GROUP BY extra1; 
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
3)What was the most common exclusion?

CREATE TEMPORARY TABLE exclusion_copy 
AS SELECT
 DISTINCT SUBSTRING_INDEX(SUBSTRING_INDEX(exclusions,',',n),',',-1) AS exclusion1, order_id
from customer_orders
CROSS JOIN (SELECT 1 AS n 
UNION 
SELECT DISTINCT 2
UNION
SELECT 3 ) numbers
WHERE SUBSTRING_INDEX(SUBSTRING_INDEX(exclusions,',',n),',',-1) REGEXP  '[0-9]' 
;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="left"> 
SELECT exclusion_copy.order_id,exclusion1, topping_name, count(exclusion1) AS most_exclusion
FROM exclusion_copy
INNER JOIN pizza_toppings
ON exclusion_copy.exclusion1 = pizza_toppings.topping_id
GROUP BY exclusion1; 
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
  <p align="center"> 
 4)Generate an order item for each record in the customers_orders table in the format of one of the following:
-- Meat Lovers
-- Meat Lovers - Exclude Beef
-- Meat Lovers - Extra Bacon
-- Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers

SELECT customer_orders.order_id, customer_id,
customer_orders.pizza_id,pizza_name,exclusions,extras,
CASE 
WHEN customer_orders.pizza_id = 1 AND (exclusions is null OR exclusions = 'null'  or exclusions = 0) 
AND (extras is NULL OR extras =0 OR extras = 'NULL')
THEN 'MEAT LOVERS'
WHEN customer_orders.pizza_id = 2 AND (exclusions is null OR exclusions = 'null'  or exclusions ='') 
AND (extras is NULL OR extras = 'NULL')
THEN 'VEG LOVERS'
WHEN customer_orders.pizza_id = 2 AND (exclusions = 4) 
AND (extras is NULL OR extras = '' OR extras = 'NULL')
THEN 'VEG LOVERS - EXCLUDE CHEESE'
WHEN customer_orders.pizza_id = 1 AND ( exclusions = 4) 
AND (extras is NULL OR extras =0 OR extras = 'NULL')
THEN 'MEAT LOVERS - EXCLUDE CHEESE'
WHEN customer_orders.pizza_id = 2 AND ( exclusions is NULL OR  exclusions = 'NULL' ) 
AND (extras REGEXP '[0-9]')
THEN 'VEG LOVERS - EXCLUDE CHEESE'
WHEN customer_orders.pizza_id = 1 AND (exclusions like '%3%' OR exclusions = 0) 
AND (extras is NULL OR extras =0 OR extras = 'NULL')
THEN 'MEAT LOVERS - EXCLUDE BEEF'
WHEN customer_orders.pizza_id = 1 AND (exclusions is null OR exclusions = 'null'  or exclusions = 0) 
AND (extras LIKE '%1%'  OR extras =1)
THEN 'MEAT LOVERS - EXTRA BACON'
WHEN customer_orders.pizza_id = 1 AND (exclusions LIKE '1,4') 
AND (extras LIKE'6,9')
THEN 'MEAT LOVERS - EXCLUDE CHEESE,BACON,-EXTRA MUSHROOM,PEPPERS'
WHEN customer_orders.pizza_id = 1 AND (exclusions LIKE '2,6') 
AND (extras LIKE'1,4')
THEN 'MEAT LOVERS - EXCLUDE BBQ,SAUCE MUSHROOM,-EXTRA BACON,CHEESE'
WHEN customer_orders.pizza_id = 1 AND (exclusions REGEXP  '[0-9]') 
AND (extras REGEXP  '[0-9]')
THEN 'MEAT LOVERS - EXCLUDE CHEESE -EXTRA BACON,CHICKEN'
END AS order_item
FROM customer_orders
JOIN pizza_names
ON pizza_names.pizza_id =customer_orders.pizza_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
5)Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients

SELECT order_id,customer_id, pizza_recipe.pizza_id, 
GROUP_CONCAT(topping_name ORDER BY topping_name SEPARATOR ',') AS standard_ingredient
FROM  customer_orders
JOIN pizza_recipe
ON pizza_recipe.pizza_id = customer_orders.pizza_id
JOIN pizza_toppings
ON pizza_recipe.topping_id = pizza_toppings.topping_id 
GROUP BY order_id, customer_id,pizza_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
7)What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?
SELECT pizza_id,runner_orders.order_id,COUNT;


 <p align="center"> 
<b>D. Pricing and Ratings</b>
<p align="center"> 
1)If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?

SELECT concat('$',
SUM(CASE WHEN pizza_id = 1 THEN 12 ELSE 10 END)) AS points
FROM customer_orders
JOIN runner_orders
on customer_orders.order_id = runner_orders.order_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<p align="center"> 
 What if there was an additional $1 charge for any pizza extras?
<p align="center"> 
Add cheese is $1 extra
SELECT concat('$',
SUM(CASE WHEN pizza_id = 1 THEN 12 
WHEN pizza_id = 2 THEN  10 
WHEN extras = 4 THEN 1 END)) AS points
FROM customer_orders
JOIN runner_orders
on customer_orders.order_id = runner_orders.order_id;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
 <p align="center"> 
 The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.
DROP TABLE IF EXISTS ratings;
CREATE TABLE ratings ( order_id integer, rating integer);
INSERT INTO ratings
( order_id, rating)
VALUES
(1,4),
(2,3),
(3,3),
(4,5),
(5,4),
(6,1),
(7,4),
(8,3),
(9,2),
(10,4);
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>

<p align="center"> 
Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?
-- customer_id
-- order_id
-- runner_id
-- rating
-- order_time
-- pickup_time
-- Time between order and pickup
-- Delivery duration
-- Average speed
-- Total number of pizzas
-- If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled - how much money does Pizza Runner have left over after these deliveries?
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
CREATE TEMPORARY TABLE all_info
AS SELECT  customer_id,customer_orders.order_id,runner_id,rating,order_time,pickup_time,
TIMEstampdiff(minute,order_time,pickup_time),
duration,
ROUND(AVG(distance/duration * 60),2),
COUNT(customer_orders.order_id)
FROM customer_orders 
INNER JOIN runner_orders
ON customer_orders.order_id = runner_orders.order_id
JOIN ratings
ON runner_orders.order_id = ratings.order_id
;
<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
