# SQL queries

## Create a table
``` sql
CREATE TABLE "customers" ("customer_id" INTEGER, "name" VARCHAR(10),
                          "age" INTEGER, "gender" VARCHAR(10),
                          "population" INTEGER);
``` 
``` sql
CREATE TABLE "customers_info" ("customer_id" INTEGER, "country" VARCHAR(10));
``` 
## Insert values into table
``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1),
(3, "Jasmine", 36, "Female", 1),
(1, "Joe", 33, "Male", 1)
``` 
``` sql
INSERT INTO customers_info values 
(1, "Canada"),
(2, "USA"),
(3, "Angleterre"),
(1, "CANADA")
``` 

## Select all columns
``` sql
SELECT *
FROM customers;
``` 

## Filter rows: where statement
``` sql
SELECT *
FROM customers
where customer_id=2;
``` 
## Select top N rows
- If N=1
``` sql
SELECT *
FROM customers
LIMIT 1;
```
## Select distinct values from column
``` sql
SELECT DISTINCT(customer_id)
FROM customers;
``` 
## Count distinct values
``` sql
SELECT COUNT(DISTINCT(customer_id))
from customers;
``` 
## Descriptive statistics
``` sql
SELECT COUNT(age),
       AVG(age),
       STDDEV(age),
       MIN(age),
       MAX(age),
       PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY age) "25%",
FROM age;
``` 
## Group by single column
``` sql
SELECT gender, SUM(population)
FROM customers
GROUP BY gender;
``` 
## Sort by single column
``` sql
SELECT *
FROM customers
ORDER BY customer_id;
``` 
## Count of unique values
``` sql
SELECT name, count(*)
FROM customers
GROUP BY name
ORDER BY COUNT(*) DESC;
```
## Drop duplicated rows (all columns are duplicated)
``` sql
SELECT DISTINCT(*)
FROM customers;
``` 
## Drop rows when a given columns value is duplicated

``` sql
DELETE  cusotmer c_1
USING
(   SELECT cusotmer_id, name
    from customer 
    QUALIFY ROW_NUMBER() OVER 
    (PARTITION BY customer_id ORDER BY cusotmer_id asc)=2
) c_2
where c_1.customer_id = c_2.customer_id and 
      c_1.name = c_2.name;
``` 
## INNER JOIN
``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1),
(3, "Jasmine", 36, "Female", 1)
``` 
``` sql
INSERT INTO customers_info values 
(1, "Canada"),
(2, "USA"),
(4, "Angleterre")
``` 
``` sql
SELECT customers.customer_id, name, country
FROM customers
INNER JOIN customer_info
ON customers.customer_id = customers_info.customer_id;
```
**Results**:
```
customer_id| name  | country
------------------------------
1          | Joe   | Canada
2          | Marc  | USA
```
## LEFT JOIN

``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1),
(3, "Jasmine", 36, "Female", 1)
``` 
``` sql
INSERT INTO customers_info values 
(1, "Canada"),
(2, "USA"),
(4, "MAROC")
``` 
``` sql
SELECT customers.customer_id, name, country
FROM customers
LEFT JOIN customer_info
ON customers.customer_id = customers_info.customer_id;
```
**Results**:
```
customer_id| name     | country
------------------------------
1          | Joe      | Canada
2          | Marc     | USA
3          | Jasmine  | null
```

## RIGHT JOIN

``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1),
(3, "Jasmine", 36, "Female", 1)
``` 
``` sql
INSERT INTO customers_info values 
(1, "Canada"),
(2, "USA"),
(4, "MAROC")
``` 
``` sql
SELECT customers.customer_id, name, country
FROM customers
RIGHT JOIN customer_info
ON customers.customer_id = customers_info.customer_id;
```
**Results**:
```
customer_id| name     | country
------------------------------
1          | Joe      | Canada
2          | Marc     | USA
4          | null     | MAROC
```
## FULL JOIN

``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1),
(3, "Jasmine", 36, "Female", 1)
``` 
``` sql
INSERT INTO customers_info values 
(1, "Canada"),
(2, "USA"),
(4, "MAROC")
``` 
``` sql
SELECT customers.customer_id, name, country
FROM customers
FULL JOIN customer_info
ON customers.customer_id = customers_info.customer_id
ORDER BY 1;
```
**Results**:
```
customer_id| name        | country
------------------------------
1          | Joe         | Canada
2          | Marc        | USA
3          | Jasmine     | null
4          | null        | MAROC
```
## CROSS JOIN

``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1)
``` 
``` sql
INSERT INTO customers_info values 
(1, "Canada"),
(2, "USA")
``` 
``` sql
SELECT *
FROM customers
CROSS JOIN customer_info;
```
**Results**:
```
customer_id| name | age|  gender|population | customer_id_2| country
---------------------------------------------------------------------
1          | Joe  | 33 |  Male  | 1         | 1            | Canada
1          | Joe  | 33 |  Male  | 1         | 2            | USA
2          | Marc | 35 |  Male  | 1         | 1            | Canada
2          | Marc | 35 |  Male  | 1         | 2            | USA
```

## UNION ALL by single column

``` sql
INSERT INTO customers values 
(1, "Joe", 33, "Male", 1),
(2, "Marc", 35, "Male", 1)
``` 
``` sql
CREATE TABLE "customers_info" ("customer_id" INTEGER, "name" VARCHAR(10));
``` 
``` sql
INSERT INTO customers_info values 
(1, "Joe"),
(4, "Doe")
``` 
``` sql
SELECT name
FROM customers
UNION ALL 
SELECT name
FROM customer_info;
```
**Results**:
```
| name | 
---------
| Joe  | 
| Marc  | 
| Joe | 
| Doe | 
```
## UNION  by single column

By using only **UNION** we will have only Joe, Jack and Doe without duplication. 


## ADD column
``` sql
ALTER TABLE "customers" ADD COLUMN address VARCHAR(10);
``` 
## UPDATE rows of table
``` sql
UPDATE TABLE "customers" set address="street one building 1" where customer_id=1;
``` 
## DROP columns
``` sql
ALTER TABLE "customers" DROP COLUMN "address", "name";
``` 
## RENAME column
``` sql
ALTER TABLE "customers" 
      RENAME COLUMN "address" to "adresse";
``` 

















