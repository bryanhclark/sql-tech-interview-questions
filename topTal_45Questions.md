# 45 Essential SQL Interview Questions *

* What does **UNION** do? What is the difference between **UNION** and **UNION ALL**?

  - **UNION** merges the contents of two compatible tables into a single table, but omits duplicate entries.
  - **UNION ALL** merges the contents of two compatible tables, but will allow duplicate entries.
  - **UNION ALL** is slightly more performant as **UNION** requires the server to delete duplicate entries.
  - In situations where there are no risk of duplicate entries, use **UNION ALL**.

* List and explain the different types of **JOIN** clauses as supported by the ANSI-standard SQL?
  - **ANSI-standard** is the American National Standards Institution

  - **INNER JOIN** (or **SIMPLE JOIN**): Returns all the rows where there is at least one match in **BOTH** tables.
    - Inner portion of Venn Diagram
  - **LEFT JOIN** or **RIGHT JOIN**: Returns all the rows from the left (or right) table and the matched rows from the right (or left) table
    - In the instance where there are no matching records in the **RIGHT** table during a **LEFT JOIN**, it will still return all the rows in the **LEFT TABLE**
  - **FULL JOIN** (or **FULL OUTER JOIN**): Returns all rows for which there is a match in either table
    - It's result is the set of a **UNION** between the **LEFT** and **RIGHT** outer queries
  - **CROSS JOIN**: Produces a result set of the rows in the first table are multiplied by the number of rows in the second table
    - This result is commonly referred to as the **Cartesian Product**

* For a table **orders** having a column defined simply as **customer_id VARCHAR(100)**, consider the following two query results:
```sql
SELECT count(*) AS total FROM orders;

+-------+
| total |
+-------+
|  100  |
+-------+

SELECT count(*) AS cust_123_total FROM orders WHERE customer_id = '123';

+----------------+
| cust_123_total |
+----------------+
|       15       |
+----------------+
```
  - Given the above query results, what would the result of the following query be?
```sql
SELECT count(*) AS cust_not_123_total FROM orders WHERE customer_id <> '123';
```
  - Obvious answer is 85, but not necessarily correct
    - If there are any null values for **customer ID**, they would not be return in the query
    - So answer is **<=85**

* What will be the result of the query below? Explain your answer and provide a version that behaves correctly.

```sql
select case when null = null then 'Yup' else 'Nope' end as Result;
```
  - In this query, the result will always be 'Nope' as the **null** cannot **= null**
    - This is because **null** is essentially **UNKNOWN** and it cannot **=** itself
  - The proper way to evaluate this query to 'Yup' is to use the **is** operator
```sql
select case when null is null then 'Yup' else 'Nope' end as Result;
```




