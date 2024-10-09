## Operational Procedure for Pivoting Techniques and String Manipulation in SQL

Pivoting and string manipulation are essential techniques for transforming and preparing data for analysis and reporting in SQL. By mastering these techniques, you can reshape your datasets and manipulate text to suit a wide range of analytical and operational needs.

#### 1. **Overview of Pivoting and String Manipulation in SQL**

Pivoting techniques and string manipulation are crucial when transforming, formatting, or restructuring data. Pivoting helps you rotate data from rows into columns, while string manipulation functions allow you to modify text-based data, including combining, extracting, and replacing values.


### 2. **Pivoting Techniques in SQL**

Pivoting in SQL allows you to transform row-based data into columns, helping to summarize or restructure datasets. This is particularly useful in reporting where you need to display data in a cross-tab format.

#### 2.1 **Basic Pivoting Example**

Pivoting is often used to display aggregated data like sales, attendance, or metrics for different time periods (months, quarters, etc.).

**Example of Pivoting Monthly Sales Data**:
```sql
SELECT product,
       SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS Jan_Sales,
       SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS Feb_Sales,
       SUM(CASE WHEN month = 'Mar' THEN sales ELSE 0 END) AS Mar_Sales
FROM sales_data
GROUP BY product;
```
- **Explanation**: This query sums the sales for each product across different months, displaying each month's total sales as a separate column.

##### 2.2 **Using the `PIVOT` Keyword (Supported in Some Databases)**
Certain databases like SQL Server and Oracle provide the `PIVOT` keyword to simplify pivot operations.

**Example Using `PIVOT`**:
```sql
SELECT product, Jan, Feb, Mar
FROM (
  SELECT product, month, sales
  FROM sales_data
) 
PIVOT (
  SUM(sales) 
  FOR month IN ('Jan', 'Feb', 'Mar')
) AS pivoted_sales;
```
- **Explanation**: This query converts the sales data into columns representing sales for January, February, and March using SQL's `PIVOT` feature.

#### 2.3 **Unpivoting Data**
Unpivoting is the reverse of pivoting and is used to convert columns back into rows.

**Example of Unpivoting Data**:
```sql
SELECT product, month, sales
FROM sales_data_unpivoted
UNPIVOT (
  sales FOR month IN (Jan_Sales, Feb_Sales, Mar_Sales)
) AS unpivoted_sales;
```
- **Explanation**: This query converts columns for monthly sales back into rows for easier analysis.


### 3. **String Manipulation Techniques in SQL**

String manipulation refers to using SQL functions to work with text data. These functions are essential for formatting, cleaning, or transforming string values, particularly in applications that involve text-heavy datasets.

#### 3.1 **Concatenation**
The `CONCAT()` function or the `||` operator (depending on the SQL dialect) allows combining multiple strings into one.

**Example of Concatenation**:
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```
- **Explanation**: This combines the first name and last name columns with a space in between to create a full name.

#### 3.2 **Substrings**
The `SUBSTRING()` function extracts a portion of a string based on specified positions.

**Example of Substring Extraction**:
```sql
SELECT SUBSTRING(phone_number, 1, 3) AS area_code
FROM customers;
```
- **Explanation**: This extracts the first three characters from the `phone_number` column, which could represent the area code.

#### 3.3 **String Replacement**
The `REPLACE()` function substitutes a specified part of a string with another string.

**Example of String Replacement**:
```sql
SELECT REPLACE(phone_number, '-', '') AS cleaned_number
FROM customers;
```
- **Explanation**: This removes dashes from the `phone_number` field, returning only the numeric digits.

#### 3.4 **Trimming Whitespace**
The `TRIM()` function removes leading and trailing spaces from a string.

**Example of Trimming Whitespace**:
```sql
SELECT TRIM(both ' ' FROM address) AS cleaned_address
FROM customers;
```
- **Explanation**: This removes unnecessary spaces from the beginning and end of the `address` field.

#### 3.5 **Upper and Lower Case**
Functions like `UPPER()` and `LOWER()` convert text to upper or lower case.

**Example of Changing Text Case**:
```sql
SELECT UPPER(first_name) AS upper_first_name
FROM employees;
```
- **Explanation**: This converts the `first_name` column to uppercase letters.


### 4. **Advanced Use Cases**

##### 4.1 **Combining Pivoting and String Manipulation**
Pivoting techniques can be combined with string manipulation for reports that require both numeric and text transformations.

**Example of Combining Pivot and CONCAT**:
```sql
SELECT product, 
       CONCAT('Sales in Jan: ', SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END)) AS Jan_Report
FROM sales_data
GROUP BY product;
```
- **Explanation**: This query creates a custom string report combining the pivoted sales data for January.


### 5. **Best Practices**

- **Use CASE Statements Wisely**: When manually pivoting data, use `CASE` statements to ensure your queries are efficient.
- **Optimize String Functions**: String functions can be slow when applied to large datasets. Consider indexing columns frequently used in string manipulations.
- **Beware of NULL Values**: String operations like concatenation can return `NULL` if any part of the concatenation involves `NULL` values. Use the `COALESCE()` function to handle `NULL`s.

---
