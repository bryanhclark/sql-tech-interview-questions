# 45 Essential SQL Interview Questions *

### 1) What does **UNION** do? What is the difference between **UNION** and **UNION ALL**?

**UNION** merges the contents of two compatible tables into a single table, but omits duplicate entries.
**UNION ALL** merges the contents of two compatible tables, but will allow duplicate entries.
**UNION ALL** is slightly more performant as **UNION** requires the server to delete duplicate entries.
In situations where there are no risk of duplicate entries, use **UNION ALL**.

### 2) List and explain the different types of **JOIN** clauses as supported by the ANSI-standard SQL?
**ANSI-standard** is the American National Standards Institution

**INNER JOIN** (or **SIMPLE JOIN**): Returns all the rows where there is at least one match in **BOTH** tables.
    - Inner portion of Venn Diagram
**LEFT JOIN** or **RIGHT JOIN**: Returns all the rows from the left (or right) table and the matched rows from the right (or left) table
  - In the instance where there are no matching records in the **RIGHT** table during a **LEFT JOIN**, it will still return all the rows in the **LEFT TABLE**
**FULL JOIN** (or **FULL OUTER JOIN**): Returns all rows for which there is a match in either table
  - It's result is the set of a **UNION** between the **LEFT** and **RIGHT** outer queries
**CROSS JOIN**: Produces a result set of the rows in the first table are multiplied by the number of rows in the second table
  - This result is commonly referred to as the **Cartesian Product**

### 3) For a table **orders** having a column defined simply as **customer_id VARCHAR(100)**, consider the following two query results:
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
Given the above query results, what would the result of the following query be?
```sql
SELECT count(*) AS cust_not_123_total FROM orders WHERE customer_id <> '123';
```
Obvious answer is 85, but not necessarily correct
  - If there are any null values for **customer ID**, they would not be return in the query
  - So answer is **<=85**

* What will be the result of the query below? Explain your answer and provide a version that behaves correctly.

```sql
select case when null = null then 'Yup' else 'Nope' end as Result;
```
In this query, the result will always be 'Nope' as the **null** cannot **= null**
  - This is because **null** is essentially **UNKNOWN** and it cannot **=** itself
The proper way to evaluate this query to 'Yup' is to use the **is** operator
```sql
select case when null is null then 'Yup' else 'Nope' end as Result;
```
### 4) Given the following Tables:
```sql
sql> SELECT * FROM runners;
+----+--------------+
| id | name         |
+----+--------------+
|  1 | John Doe     |
|  2 | Jane Doe     |
|  3 | Alice Jones  |
|  4 | Bobby Louis  |
|  5 | Lisa Romero  |
+----+--------------+

sql> SELECT * FROM races;
+----+----------------+-----------+
| id | event          | winner_id |
+----+----------------+-----------+
|  1 | 100 meter dash |  2        |
|  2 | 500 meter dash |  3        |
|  3 | cross-country  |  2        |
|  4 | triathalon     |  NULL     |
+----+----------------+-----------+
```
What will the result of the following query be:
```sql
SELECT * FROM runners WHERE id NOT IN (SELECT winner_id FROM races)
```
The answer will be **FALSE**
  - If the condition being evaluated by SQL **NOT IN** contains any **null** values, then the outer query will always return an empty set
Fixed Query:
```sql
SELECT * FROM runners WHERE id NOT IN (SELECT winner_id from races WHERE winner_id IS NOT null)
```

### 5) Given two tables created and populated as follows:

```sql
CREATE TABLE dbo.envelope(id int, user_id int);
CREATE TABLE dbo.docs(idnum int, pageseq int, doctext varchar(100));

INSERT INTO dbo.envelope VALUES
  (1,1),
  (2,2),
  (3,3);

INSERT INTO dbo.docs(idnum,pageseq) VALUES
  (1,5),
  (2,6),
  (null,0);
```

What will the result of the following query?

```sql
UPDATE docs SET doctext=pageseq FROM docs INNER JOIN envelope ON envelope.id=docs.idnum
WHERE EXISTS (
  SELECT 1 FROM dbo.docs
  WHERE id=envelope.id
);
```
The result of the query would be:
```sql
idnum  pageseq  doctext
1      5        5
2      6        6
NULL   0        NULL
```

The **EXIST** clause in the query is the red flag:
  - It will always return true since **ID** is not a member of db.docs
  - It will refer to the envelope table, comparing itself to itself
  - The **idnum** of **null** will not be set since the join of **null** will not return a result when attempting to match with any value from envelope

### 6) What is Wrong with this SQL Query, correct it so it executes correctly?

```sql
SELECT Id, YEAR(BillingDate) AS BillingYear 
FROM Invoices
WHERE BillingYear >= 2010;
```
The Expression BillingYear in the WHERE clause is incorrect.
  - Even though it was defined as an alias in the **SELECT**, SQL executes **WHERE** clauses before assigning alias'

Correct Query:
```sql
SELECT Id, YEAR(BillingDate) AS BillingYear
FROM Invoices
WHERE YEAR(BillingDate) >= 2010;
```

### 7) Given these contents of the Customers table:

```sql
Id	Name			ReferredBy
1	John Doe		NULL
2	Jane Smith		NULL
3	Anne Jenkins		2
4	Eric Branford		NULL
5	Pat Richards		1
6	Alice Barnes		2
```
Here is a query written to return the list of customers not referred by Jane Smith:
```sql
SELECT Name FROM Customers WHERE ReferredBy <> 2;
```
What will be the result of the query? Why? What would be a better way to write it?
 - The Result would just be **Pat Richards** as the **WHERE** clause will not return **null** values
 - Anything compared to **NULL** evaluates to **UNKNOWN**
  - There are 3 logic values:
    - True
    - False
    - UNKNOWN
A correct query would be:
```sql
SELECT Name FROM Customers WHERE ReferredBy IS NULL OR ReferredBy <> 2;
```



