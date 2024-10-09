# Operacional Procedure – Window Functions in SQL

This manual covers the most important concepts related to SQL window functions. For more technical details and examples, refer to the official documentation from [PostgreSQL](https://www.postgresql.org/docs/current/tutorial-window.html) or [Oracle SQL](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/SQL-Window-Functions.html).

#### 1. **What Are Window Functions?**
Window functions in SQL allow you to perform calculations across a set of rows related to the result of a query without needing to group the data. They are useful when you want to aggregate metrics while retaining detailed context for each row.


#### 2. **Basic Syntax**
The general structure of a window function is:

```sql
<window_function>() OVER (
    [PARTITION BY <column>]
    [ORDER BY <column>]
    [frame_spec]
)
```

- **PARTITION BY**: Divides the rows into subsets (partitions) where the function is applied separately.
- **ORDER BY**: Specifies the order in which rows are processed within each partition.
- **Frame Specification (Optional)**: Defines a specific subset of rows relative to the current row.

#### 3. **Key Window Functions**
##### 3.1 **ROW_NUMBER()**
Assigns a unique row number within a partition of a result set.

Example:
```sql
SELECT customer_name, total_purchase,
       ROW_NUMBER() OVER (ORDER BY total_purchase DESC) AS rank
FROM sales;
```

##### 3.2 **RANK()**
Assigns a ranking to rows based on the specified order. Rows with equal values get the same rank, and there is a gap in the numbering.

Example:
```sql
SELECT customer_name, total_purchase,
       RANK() OVER (ORDER BY total_purchase DESC) AS rank
FROM sales;
```

##### 3.3 **DENSE_RANK()**
Similar to `RANK()`, but without gaps in the ranking numbers.

Example:
```sql
SELECT customer_name, total_purchase,
       DENSE_RANK() OVER (ORDER BY total_purchase DESC) AS rank
FROM sales;
```

##### 3.4 **LAG() and LEAD()**
- **LAG()**: Retrieves the value from the previous row.
- **LEAD()**: Retrieves the value from the next row.

Example:
```sql
SELECT product_name, sales,
       LAG(sales, 1) OVER (ORDER BY sale_date) AS previous_sales
FROM sales;
```

#### 4. **Defining Frame Specifications**
Frame specifications allow you to control the rows considered by the window function. You can define ranges like:

- **ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW**: Considers all rows up to the current one.
- **ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING**: Considers the previous row, current row, and the next row.

Example:
```sql
SELECT product, sales,
       SUM(sales) OVER (ORDER BY sale_date
                         ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS total_sales
FROM sales;
```


#### 5. **Practical Examples**
##### 5.1 **Calculating Moving Averages**
You can use window functions to calculate moving averages.

Example:
```sql
SELECT sale_date, sales,
       AVG(sales) OVER (ORDER BY sale_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
FROM sales;
```

##### 5.2 **Ranking Results Within Groups**
You can use `RANK()` or `ROW_NUMBER()` to assign rankings within specific groups.

Example:
```sql
SELECT product_category, product_name, sales,
       RANK() OVER (PARTITION BY product_category ORDER BY sales DESC) AS product_rank
FROM sales;
```


#### 6. **Best Practices**
- **Use PARTITION BY Carefully**: Properly divide data into logical partitions to avoid incorrect calculations.
- **Monitor Performance**: Window functions can be resource-intensive on large datasets. Optimize queries by using appropriate indexes.
- **Understand Frame Specifications**: Use frame specifications properly to get the correct results.
