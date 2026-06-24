E-COMMERCE BUSINESS ANALYSIS PROJECT

Dataset: Online Retail II (2009-2011)

Database: PostgreSQL



Objective:

1\. Extract Data

2\. Clean Data

3\. Join Tables

4\. Calculate KPIs

5\. Generate Business Insights



Author: Arnold Muia





1\. DATA EXPLORATION





\-- Preview invoice data

SELECT \*

FROM invoice

LIMIT 10;



\-- Preview customer data

SELECT \*

FROM customers

LIMIT 10;



\-- Preview product data

SELECT \*

FROM products

LIMIT 10;





\-- Row counts



SELECT COUNT(\*) AS total\_transactions

FROM invoice;



SELECT COUNT(\*) AS total\_customer\_rows

FROM customers;



SELECT COUNT(\*) AS total\_product\_rows

FROM products;





2\. DATA QUALITY CHECKS





\-- Check NULL values in invoice table



SELECT

&#x20;   COUNT(\*) AS total\_rows,



&#x20;   COUNT(invoiceid) AS invoiceid\_non\_null,

&#x20;   COUNT(\*) - COUNT(invoiceid) AS invoiceid\_nulls,



&#x20;   COUNT(customerid) AS customerid\_non\_null,

&#x20;   COUNT(\*) - COUNT(customerid) AS customerid\_nulls,



&#x20;   COUNT(invoice\_date) AS invoice\_date\_non\_null,

&#x20;   COUNT(\*) - COUNT(invoice\_date) AS invoice\_date\_nulls,



&#x20;   COUNT(quantity) AS quantity\_non\_null,

&#x20;   COUNT(\*) - COUNT(quantity) AS quantity\_nulls,



&#x20;   COUNT(price) AS price\_non\_null,

&#x20;   COUNT(\*) - COUNT(price) AS price\_nulls



FROM invoice;





\-- Check duplicate invoices



SELECT

&#x20;   invoiceid,

&#x20;   COUNT(\*) AS duplicate\_count

FROM invoice

GROUP BY invoiceid

HAVING COUNT(\*) > 1

ORDER BY duplicate\_count DESC;





\-- Check invalid quantities



SELECT \*

FROM invoice

WHERE quantity <= 0;





\-- Check invalid prices



SELECT \*

FROM invoice

WHERE price <= 0;





3\. DATA PREPARATION





\-- Create analysis dataset



SELECT

&#x20;   i.invoiceid,

&#x20;   i.customerid,

&#x20;   c.country,

&#x20;   p.productid,

&#x20;   p.description,

&#x20;   i.invoice\_date,

&#x20;   i.quantity,

&#x20;   i.price,

&#x20;   (i.quantity \* i.price) AS revenue

FROM invoice i

LEFT JOIN customers c

&#x20;   ON i.invoiceid = c.invoiceid

LEFT JOIN products p

&#x20;   ON i.invoiceid = p.invoiceid;





4\. CORE BUSINESS KPIs





\-- Total Revenue



SELECT

&#x20;   ROUND(SUM(quantity \* price),2) AS total\_revenue

FROM invoice;





\-- Total Transactions



SELECT

&#x20;   COUNT(\*) AS total\_transactions

FROM invoice;





\-- Total Customers



SELECT

&#x20;   COUNT(DISTINCT customerid) AS total\_customers

FROM customers;





\-- Average Order Value (AOV)



SELECT

&#x20;   ROUND(AVG(quantity \* price),2) AS average\_order\_value

FROM invoice;





\-- Modal Order Value (MOV)



SELECT

&#x20;   MODE() WITHIN GROUP (ORDER BY quantity \* price)

&#x20;   AS modal\_order\_value

FROM invoice;





5\. BUSINESS QUESTION #1

WHICH PRODUCTS GENERATE THE MOST REVENUE?



**Most sold product per year:**

WITH yearly\_product\_sales AS (

&#x20;   SELECT

&#x20;       EXTRACT(YEAR FROM i.invoice\_date) AS sales\_year,

&#x20;       p.description AS product\_name,

&#x20;       SUM(i.quantity) AS total\_quantity

&#x20;   FROM products p

&#x20;   JOIN invoice i ON p.invoiceID = i.invoiceID

&#x20;   GROUP BY sales\_year, p.description

),

ranked\_products AS (

&#x20;   SELECT

&#x20;       sales\_year,

&#x20;       product\_name,

&#x20;       total\_quantity,

&#x20;       ROW\_NUMBER() OVER (PARTITION BY sales\_year ORDER BY total\_quantity DESC) as rank

&#x20;   FROM yearly\_product\_sales

)

SELECT

&#x20;   sales\_year,

&#x20;   product\_name,

&#x20;   total\_quantity

FROM ranked\_products

WHERE rank = 1

ORDER BY sales\_year;



Findings:

White hanging heart t-light holder Quantity (95474) Year (2009)

White hanging heart t-light holder Quantity (116048) Year (2010)

jumbo bag red retrospot Quantity (890988) Year(2011)





6\. BUSINESS QUESTION #2

WHICH CUSTOMERS SPEND THE MOST?



**Customers spend the most in each year:**

WITH yearly\_customer\_spending AS (

&#x20;   SELECT

&#x20;       EXTRACT(YEAR FROM i.invoice\_date) AS sales\_year,

&#x20;       c.customerid,

&#x20;       c.country,

&#x20;       SUM(i.quantity \* i.price) AS total\_spent

&#x20;   FROM customers c

&#x20;   JOIN invoice i ON c.invoiceID = i.invoiceID

&#x20;   GROUP BY sales\_year, c.customerid, c.country

),

ranked\_customers AS (

&#x20;   SELECT

&#x20;       sales\_year,

&#x20;       customerid,

&#x20;       country,

&#x20;       total\_spent,

&#x20;       ROW\_NUMBER() OVER (PARTITION BY sales\_year ORDER BY total\_spent DESC) as rank

&#x20;   FROM yearly\_customer\_spending

)

SELECT

&#x20;   sales\_year,

&#x20;   customerid,

&#x20;   country,

&#x20;   total\_spent

FROM ranked\_customers

WHERE rank <= 3

ORDER BY sales\_year, rank;



Findings:

sales\_year	customerid	country	total\_spent

2009	NaN	United Kingdom	57857611.43

2009	13694	United Kingdom	1601830.97

2009	15823	United Kingdom	580915.41

2010	NaN	United Kingdom	412790886.7

2010	14646	Netherlands	11880468.05

2010	14156	EIRE	9522222.72

2011	NaN	United Kingdom	347073031.4

2011	14096	United Kingdom	28532583.09

2011	14646	Netherlands	17534913.73





7\. BUSINESS QUESTION #3

WHICH REGIONS (COUNTRIES) PERFORM BEST?



SELECT

&#x20;   c.country,

&#x20;   ROUND(SUM(i.quantity \* i.price),2) AS total\_revenue,

&#x20;   COUNT(DISTINCT c.customerid) AS customers

FROM customers c

JOIN invoice i

&#x20;   ON c.invoiceid = i.invoiceid

GROUP BY c.country

ORDER BY total\_revenue DESC;





8\. BUSINESS QUESTION #4

WHICH MONTHS HAVE THE HIGHEST SALES?



**Months with Highest revenue:**

WITH monthly\_sales AS (

&#x20;   SELECT

&#x20;       EXTRACT(YEAR FROM i.invoice\_date) AS sales\_year,

&#x20;       EXTRACT(MONTH FROM i.invoice\_date) AS sales\_month,

&#x20;       SUM(i.quantity \* i.price) AS total\_revenue

&#x20;   FROM products p

&#x20;   JOIN invoice i ON p.invoiceID = i.invoiceID

&#x20;   GROUP BY sales\_year, sales\_month

),

ranked\_sales AS (

&#x20;   SELECT

&#x20;       sales\_year,

&#x20;       sales\_month,

&#x20;       total\_revenue,

&#x20;       ROW\_NUMBER() OVER (PARTITION BY sales\_year ORDER BY total\_revenue DESC) as rank

&#x20;   FROM monthly\_sales

)

SELECT

&#x20;   sales\_year,

&#x20;   sales\_month,

&#x20;   total\_revenue

FROM ranked\_sales

WHERE rank = 1

ORDER BY sales\_year;



Findings:

2010 Dec (256663166.72), 2011 Nov (150292728.33), 2009 Dec (77475021)





KEY BUSINESS INSIGHTS





1\. Revenue peaked in 2010.



2\. December is the strongest sales month,

&#x20;  indicating significant holiday-season demand.



3\. White Hanging Heart T-Light Holder dominated

&#x20;  product sales in 2009 and 2010.



4\. Jumbo Bag Red Retrospot became the best-selling

&#x20;  product in 2011.



5\. Rabbit Night Light was the top-selling product

&#x20;  during the highest revenue month of 2011.



6\. The United Kingdom contributes the majority

&#x20;  of revenue and customer activity.



7\. Guest(NAN) purchases account for a significant

&#x20;  proportion of revenue, suggesting opportunities

&#x20;  for customer registration campaigns.



8\. Registered customers have an average order

&#x20;  value of approximately £18.07.



