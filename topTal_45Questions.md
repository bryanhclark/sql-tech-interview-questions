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



