`# SQL questionnaire

## Setup

Import this [fake database content](mysqlsampledatabase.zip) into your database.

Below you will find a list of questions.

Find out the answer to each question using the dummy data in your database.

**Copy this file** (see: copy raw content) and fill in your queries + answer on the given location in each question.

## START !

### 1) How many customers do we have?

```
SELECT count(customerNumber) FROM customers;
```

solution: `122`

### 2) What is the customer number of Mary Young?

```
SELECT customerNumber FROM customers
WHERE contactFirstName = 'Mary' AND contactLastName = 'Young';
```

solution: `219`

### 3) What is the customer number of the person living at Magazinweg 7, Frankfurt 60528?

```
SELECT customerNumber FROM customers
WHERE addressLine1 = 'Magazinweg 7' AND city = 'Frankfurt' AND postalCode = '60528';
```

solution: `247`

### 4) If you sort the employees on their last name, what is the email of the first employee?

```
SELECT email FROM employees
ORDER BY lastName ASC LIMIT 1;
```

solution: `gbondur@classicmodelcars.com`

### 5) If you sort the employees on their last name, what is the email of the last employee?

```
SELECT	email FROM employees
ORDER BY lastName DESC LIMIT 1;
```

solution: `gvanauf@classicmodelcars.com`

### 6) What is first the product code of all the products from the line 'Trucks and Buses', sorted first by productscale, then by productname.

```
SELECT productCode FROM products
WHERE productLine = 'Trucks and Buses'
ORDER BY productScale, productName LIMIT 1;
```

solution: `S12_4473`

### 7) What is the email of the first employee, sorted on their last name that starts with a 't'?

```
SELECT email FROM employees
WHERE lastName LIKE 't%'
ORDER BY lastName ASC LIMIT 1;
```

solution: `lthompson@classicmodelcars.com`

### 8) Which customer (give customer number) payed by check on 2004-01-19?

```
SELECT customerNumber FROM payments
WHERE paymentDate = '2004-01-19';
```

solution: `177`

### 9) How many customers do we have living in the state Nevada or New York?

```
SELECT count(customerNumber) FROM customers
WHERE  state in  ("Nevada", "New York");
```

solution: `0`

### 10) How many customers do we have living in the state Nevada or New York or outside the united states?

```
SELECT count(customerNumber) FROM customers
WHERE  state in  ("Nevada", "New York") OR country !="USA";
```

solution: `86`

### 11) How many customers do we have with the following conditions (only 1 query needed): - Living in the state Nevada or New York OR - Living outside the USA or the customers and with a credit limit above 1000 dollar?

```
SELECT count(customerNumber) FROM customers
WHERE  state in  ("Nevada", "New York") OR country !="USA" AND creditLimit > 1000;
```

solution: `63`

### 12) How many customers don't have an assigned sales representative?

```
SELECT count(customerNumber) FROM customers
WHERE salesRepEmployeeNumber IS NULL;

```

solution: `22`

### 13) How many orders have a comment?

```
SELECT count(orderNumber) FROM orders
WHERE comments IS NOT NULL;

```

solution: `80`

### 14) How many orders do we have where the comment mentions the word "caution"?

```
SELECT COUNT(orderNumber) FROM orders
WHERE comments LIKE '%caution%';
```

solution: `4`

### 15) What is the average credit limit of our customers from the USA? (rounded)

```
SELECT ROUND(AVG(creditLimit)) FROM customers
WHERE country = 'USA';
```

solution: `78103`

### 16) What is the most common last name from our customers?

```
SELECT contactLastName, COUNT(contactLastName) AS count FROM customers
GROUP BY contactLastName
ORDER BY count DESC LIMIT 1;
```

solution: `Young`

### 17) What are valid statuses of the orders?

- [x] Resolved

- [x] Cancelled

- [ ] Broken

- [x] On Hold

- [x] Disputed

- [x] In Process

- [ ] Processing

- [x] Shipped

```
SELECT DISTINCT status FROM orders;
```

solution: `
Shipped   |
Resolved  |
Cancelled |
On Hold   |
Disputed  |
In Process|`

### 18) In which countries don't we have any customers?

- [ ] Austria

- [ ] Canada

- [ ] China

- [ ] Germany

- [ ] Greece

- [ ] Japan

- [ ] Philippines

- [ ] South Korea

```
<Your SQL query` here>
```

solution: `<your solution here>`

### 19) How many orders where never shipped?

```
SELECT COUNT(*)
FROM orders
WHERE status != 'Shipped';
```

solution: `23`

### 20) How many customers does Steve Patterson have with a credit limit above 100 000 EUR?

```
SELECT COUNT(*)
FROM customers
WHERE salesRepEmployeeNumber = (SELECT employeeNumber FROM employees WHERE firstName = 'Steve' AND lastName = 'Patterson')
AND creditLimit > 100000;
```

solution: `3`

### 21) How many orders have been shipped to our customers?

```
SELECT COUNT(*)
FROM orders
WHERE status = 'Shipped';
```

solution: `303`

### 22) How much products does the biggest product line have? And which product line is that?

```
SELECT productLine, COUNT(*) as count
FROM products
GROUP BY productLine
ORDER BY count DESC
LIMIT 1;
```

solution: `Classic Cars|   38|`

### 23) How many products are low in stock? (below 100 pieces)

```
SELECT COUNT(*)
FROM products
WHERE quantityInStock < 100;
```

solution: `2`

### 24) How many products have more the 100 pieces in stock, but are below 500 pieces.

```
SELECT COUNT(*)
FROM products
WHERE quantityInStock BETWEEN 101 AND 499;
```

solution: `3`

### 25) How many orders did we ship between and including June 2004 & September 2004

```
SELECT COUNT(*)
FROM orders
WHERE orderDate BETWEEN '2004-06-01' AND '2004-09-30';
```

solution: `47`

### 26) How many customers share the same last name as an employee of ours?

```
SELECT COUNT(*)
FROM customers
WHERE contactLastName IN (SELECT lastName FROM employees);
```

solution: `9`

### 27) Give the product code for the most expensive product for the consumer?

```
SELECT productCode
FROM products
ORDER BY MSRP DESC
LIMIT 1;
```

solution: `S10_1949`

### 28) What product (product code) offers us the largest profit margin (difference between buyPrice & MSRP).

```
SELECT productCode, (MSRP - buyPrice) as profit_margin
FROM products
ORDER BY profit_margin DESC
LIMIT 1;
```

solution: `S10_1949	115.72`

### 29) How much profit (rounded) can the product with the largest profit margin (difference between buyPrice & MSRP) bring us.

```
SELECT ROUND((MSRP - buyPrice)) as profit_margin
FROM products
ORDER BY profit_margin DESC
LIMIT 1;
```

solution: `116`

### 30) Given the product number of the products (separated with spaces) that have never been sold?

```
SELECT productCode
FROM products
WHERE productCode NOT IN (SELECT productCode FROM orderdetails);
```

solution: `S18_3233`

### 31) How many products give us a profit margin below 30 dollar?

```
SELECT COUNT(*)
FROM products
WHERE (MSRP - buyPrice) < 30;
```

solution: `23`

### 32) What is the product code of our most popular product (in number purchased)?

```
SELECT productCode
FROM orderdetails
GROUP BY productCode
ORDER BY SUM(quantityOrdered) DESC
LIMIT 1;
```

solution: `S18_3232`

### 33) How many of our popular product did we effectively ship?

```
SELECT SUM(quantityOrdered)
FROM orderdetails
WHERE productCode = (
    SELECT productCode
    FROM orderdetails
    GROUP BY productCode
    ORDER BY SUM(quantityOrdered) DESC
    LIMIT 1
);
```

solution: `1808`

### 34) Which check number paid for order 10210. Tip: Pay close attention to the date fields on both tables to solve this.

```
SELECT checkNumber
FROM payments
WHERE customerNumber = (
    SELECT customerNumber
    FROM orders
    WHERE orderNumber = 10210
);
```

solution: `AU750837  
CI381435  `

### 35) Which order was paid by check CP804873?

```
SELECT orderNumber
FROM orders
WHERE customerNumber = (
    SELECT customerNumber
    FROM payments
    WHERE checkNumber = 'CP804873'
);
```

solution: `      10108|
      10198|
      10330|`

### 36) How many payments do we have above 5000 EUR and with a check number with a 'D' somewhere in the check number, ending the check number with the digit 9?

```
SELECT COUNT(*)
FROM payments
WHERE amount > 5000 AND checkNumber LIKE '%D%' AND checkNumber LIKE '%9';
```

solution: `3`

### 38) In which country do we have the most customers that we do not have an office in?

```
SELECT country, COUNT(*) as count
FROM customers
WHERE country NOT IN (SELECT country FROM offices)
GROUP BY country
ORDER BY count DESC
LIMIT 1;
```

solution: `Germany|   13|`

### 39) What city has our biggest office in terms of employees?

```
SELECT city, COUNT(*) as count
FROM employees
JOIN offices ON employees.officeCode = offices.officeCode
GROUP BY city
ORDER BY count DESC
LIMIT 1;
```

solution: `San Francisco|    6|`

### 40) How many employees does our largest office have, including leadership?

```
SELECT COUNT(*)
FROM employees
WHERE officeCode = (
    SELECT officeCode
    FROM employees
    GROUP BY officeCode
    ORDER BY COUNT(*) DESC
    LIMIT 1
);

```

solution: `6`

### 41) How many employees do we have on average per country (rounded)?

```
SELECT ROUND(AVG(emp_count))
FROM (
    SELECT COUNT(*) as emp_count
    FROM employees
    JOIN offices ON employees.officeCode = offices.officeCode
    GROUP BY country
) sub_query;
```

solution: `5`

### 42) What is the total value of all shipped & resolved sales ever combined?

```
SELECT SUM(quantityOrdered * priceEach)
FROM orderdetails
JOIN orders ON orderdetails.orderNumber = orders.orderNumber
WHERE status IN ('Shipped', 'Resolved');
```

solution: ` 8999330.52`

### 43) What is the total value of all shipped & resolved sales in the year 2005 combined? (based on shipping date)

```
SELECT SUM(quantityOrdered * priceEach)
FROM orderdetails
JOIN orders ON orderdetails.orderNumber = orders.orderNumber
WHERE YEAR(shippedDate) = 2005 AND status IN ('Shipped', 'Resolved');
```

solution: ` 1427944.97`

### 44) What was our most profitable year ever (based on shipping date), considering all shipped & resolved orders?

```
SELECT YEAR(shippedDate) as year, SUM(quantityOrdered * priceEach) as total_profit
FROM orderdetails
JOIN orders ON orderdetails.orderNumber = orders.orderNumber
WHERE status IN ('Shipped', 'Resolved')
GROUP BY year
ORDER BY total_profit DESC
LIMIT 1;
```

solution: `2004|  4321167.85|`

### 45) How much revenue did we make on in our most profitable year ever (based on shipping date), considering all shipped & resolved orders?

```
SELECT SUM(quantityOrdered * priceEach) as total_profit
FROM orderdetails
JOIN orders ON orderdetails.orderNumber = orders.orderNumber
WHERE YEAR(shippedDate) = (
    SELECT YEAR(shippedDate)
    FROM orderdetails
    JOIN orders ON orderdetails.orderNumber = orders.orderNumber
    WHERE status IN ('Shipped', 'Resolved')
    GROUP BY YEAR(shippedDate)
    ORDER BY SUM(quantityOrdered * priceEach) DESC
    LIMIT 1
);
```

solution: `  4366611.39|`

### 46) What is the name of our biggest customer in the USA of terms of revenue?

```
SELECT customerName, SUM(quantityOrdered * priceEach) as total_revenue
FROM customers
JOIN orders ON customers.customerNumber = orders.customerNumber
JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
WHERE country = 'USA'
GROUP BY customerName
ORDER BY total_revenue DESC
LIMIT 1;
```

solution: `Mini Gifts Distributors Ltd.|    591827.34|`

### 47) How much has our largest customer inside the USA ordered with us (total value)?

```
SELECT SUM(quantityOrdered * priceEach) as total_order_value
FROM customers
JOIN orders ON customers.customerNumber = orders.customerNumber
JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
WHERE country = 'USA' AND customerName = (
    SELECT customerName
    FROM customers
    JOIN orders ON customers.customerNumber = orders.customerNumber
    JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
    WHERE country = 'USA'
    GROUP BY customerName
    ORDER BY SUM(quantityOrdered * priceEach) DESC
    LIMIT 1
);
```

solution: ` 591827.34|`

### 48) How many customers do we have that never ordered anything?

```
SELECT COUNT(*)
FROM customers
WHERE customerNumber NOT IN (SELECT DISTINCT customerNumber FROM orders);
```

solution: `   24`

### 49) What is the last name of our best employee in terms of revenue?

```
SELECT lastName, SUM(quantityOrdered * priceEach) as total_revenue
FROM employees
JOIN customers ON employees.employeeNumber = customers.salesRepEmployeeNumber
JOIN orders ON customers.customerNumber = orders.customerNumber
JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
GROUP BY lastName
ORDER BY total_revenue DESC
LIMIT 1;
```

solution: `Hernandez|   1258577.81|`

### 50) What is the office name of the least profitable office in the year 2004?

```
SELECT offices.city
FROM offices
JOIN employees ON offices.officeCode = employees.officeCode
JOIN customers ON employees.employeeNumber = customers.salesRepEmployeeNumber
JOIN orders ON customers.customerNumber = orders.customerNumber
JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
WHERE YEAR(orders.orderDate) = 2004
GROUP BY offices.city
ORDER BY SUM(orderdetails.quantityOrdered * orderdetails.priceEach)
LIMIT 1;

```

solution: `Tokyo`

## Are you done? Amazing!

![](../_assets/clap-clap-clap.gif)
