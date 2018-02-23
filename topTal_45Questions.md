# 45 Essential SQL Interview Questions *

* What does **UNION** do? What is the difference between **UNION** and **UNION ALL**?

  - **UNION** merges the contents of two compatible tables into a single table, but omits duplicate entries.
  - **UNION ALL** merges the contents of two compatible tables, but will allow duplicate entries.
  - **UNION ALL** is slightly more performant as **UNION** requires the server to delete duplicate entries.
  - In situations where there are no risk of duplicate entries, use **UNION ALL**.




