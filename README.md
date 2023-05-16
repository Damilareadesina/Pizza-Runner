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
