# :ramen: Case Study #1: Danny's Diner

## Case Study Questions

1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
***

####  1. What is the total amount each customer spent at the restaurant?
````sql
SELECT 
    s.customer_id, 
    SUM(m.price) AS 'amount spend'
FROM
    sales AS s
        INNER JOIN
    menu AS m USING (product_id)
GROUP BY s.customer_id;
````
![image](https://user-images.githubusercontent.com/66305714/211383164-6ace6cbe-f203-417e-b04e-4a6a5e9c8355.png)

#### 2. How many days has each customer visited the restaurant?
````sql
SELECT 
    s.customer_id,
    COUNT(DISTINCT s.order_date) AS 'num of visits'
FROM
    sales AS s
        INNER JOIN
    menu AS m USING (product_id)
````
![image](https://user-images.githubusercontent.com/66305714/211383377-d0d067e4-e0a7-44f0-9437-de74635159ba.png)

#### 3. What was the first item from the menu purchased by each customer?
````sql
WITH order_details AS (
	SELECT 
		s.customer_id, 
		m.product_name, 
    DENSE_RANK() OVER(PARTITION BY s.customer_id ORDER BY order_date ASC) as rank_num
	FROM 
		sales AS s
			INNER JOIN  
		menu AS m USING(product_id)
)
SELECT 
    customer_id,
    GROUP_CONCAT(DISTINCT product_name
        ORDER BY product_name) AS first_purchased_item
FROM
    order_details
WHERE
    rank_num = 1
GROUP BY customer_id;
````
![image](https://user-images.githubusercontent.com/66305714/211382212-07270e2d-c9fd-4718-96e6-d57ffab8872f.png)

#### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
````sql
SELECT 
    m.product_name AS most_purchased,
    COUNT(product_id) AS no_of_orders
FROM
    menu m
    INNER JOIN
    sales s USING (product_id)
GROUP BY product_id
ORDER BY no_of_orders DESC
LIMIT 1;
````
![image](https://user-images.githubusercontent.com/66305714/211386000-cb79b49b-a7ce-4d13-b0cc-258015eaea49.png)

#### 5. Which item was the most popular for each customer?
````sql
````
####  6. Which item was purchased first by the customer after they became a member?
````sql
SELECT 
    s.customer_id, SUM(m.price) AS 'amount spend'
FROM
    sales AS s
        INNER JOIN
    menu AS m USING (product_id)
GROUP BY s.customer_id;
````
#### 7. Which item was purchased just before the customer became a member?
````sql
````
#### 8. What is the total items and amount spent for each member before they became a member?
````sql
````
#### 9.  If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
````sql
````
#### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January
````sql
````
