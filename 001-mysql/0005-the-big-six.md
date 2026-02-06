# The Big Six of SQL — The Core Building Blocks of Every Query

At the heart of SQL lies a small set of keywords that form the foundation of almost every query written in real-world systems. These six clauses define **what data is selected**, **where it comes from**, **how it is filtered**, **how it is grouped**, **how groups are filtered**, and **how results are ordered**.

These six clauses are commonly referred to as **“The Big Six”**. Understanding them deeply — including their order, behavior, and edge cases — is essential to writing correct and efficient SQL.

---

## 1. SELECT — Choosing What Data You Want

The `SELECT` clause defines **which columns appear in the final result set**. It does not modify data; it only determines what is returned.

Basic syntax:

    SELECT column_name

Example:

    SELECT first_name
    FROM customers;

This query returns only the `first_name` column from the `customers` table.

### Selecting Multiple Columns

    SELECT first_name, last_name, email
    FROM customers;

Columns are returned in the exact order listed in the `SELECT` clause.

### Common Gotchas with SELECT

- `SELECT *` returns all columns but is discouraged in production because:
  - It can return unnecessary data
  - It breaks queries if table structure changes
- Column aliases do not exist until after `SELECT` runs, so they cannot be used in `WHERE`

---

## 2. FROM — Identifying the Data Source

The `FROM` clause specifies **which table (or tables)** the query pulls data from.

Basic syntax:

    FROM table_name

Example:

    SELECT *
    FROM orders;

Without `FROM`, SQL has no idea where the data lives. Every meaningful query must include a `FROM` clause.

### Core Rule

Every valid query contains at minimum:

    SELECT column_name
    FROM table_name

### Gotchas with FROM

- Table names must exist in the database schema
- If multiple tables are used (via joins), `FROM` defines the starting point

---

## 3. WHERE — Filtering Individual Rows

The `WHERE` clause filters **rows before grouping occurs**. It operates at the **row level**, not the group level.

Basic syntax:

    WHERE logical_condition

Example:

    SELECT *
    FROM customers
    WHERE country = 'India';

Only rows matching the condition are returned.

### Multiple Conditions

    SELECT *
    FROM orders
    WHERE amount > 100
      AND status = 'completed';

### Common WHERE Gotchas

- `WHERE` cannot use aggregate functions like `COUNT`, `SUM`, or `AVG`
- `WHERE` runs before `GROUP BY`
- `NULL` comparisons require `IS NULL` or `IS NOT NULL`, not `=`

---

## 4. GROUP BY — Grouping Rows into Buckets

The `GROUP BY` clause groups rows that share the same value into a single group. It is used together with aggregate functions.

Basic syntax:

    GROUP BY column_name

Example:

    SELECT country, COUNT(*)
    FROM customers
    GROUP BY country;

This query groups all customers by country and counts how many exist in each group.

### Key Rule of GROUP BY

Every column in `SELECT` must be either:
- Listed in `GROUP BY`, or
- Wrapped in an aggregate function

Invalid example:

    SELECT country, email
    FROM customers
    GROUP BY country;

This fails because `email` is neither grouped nor aggregated.

### GROUP BY Gotchas

- Grouping changes the meaning of the result set
- You no longer work with rows, but with groups
- `GROUP BY` always runs after `WHERE`

---

## 5. HAVING — Filtering Groups

The `HAVING` clause filters **groups**, not rows. It is applied **after** `GROUP BY`.

Basic syntax:

    HAVING logical_condition

Example:

    SELECT country, COUNT(*)
    FROM customers
    GROUP BY country
    HAVING COUNT(*) > 10;

This returns only countries that have more than 10 customers.

### HAVING vs WHERE (Critical Difference)

- `WHERE` filters rows before grouping
- `HAVING` filters groups after grouping

Invalid usage:

    SELECT country, COUNT(*)
    FROM customers
    WHERE COUNT(*) > 10
    GROUP BY country;

This fails because `WHERE` cannot use aggregates.

### HAVING Gotchas

- `HAVING` cannot exist without `GROUP BY`
- Use `HAVING` only for conditions involving aggregates

---

## 6. ORDER BY — Sorting the Results

The `ORDER BY` clause defines **how the final result set is sorted**. It always appears last in the query.

Basic syntax:

    ORDER BY column_name

Example:

    SELECT *
    FROM customers
    ORDER BY last_name;

### Ascending and Descending Order

    ORDER BY created_at DESC;

Default order is ascending (`ASC`) if not specified.

### ORDER BY Gotchas

- Sorting happens after all filtering and grouping
- `ORDER BY` can use column aliases
- Ordering large result sets can be expensive

---

## The Required Order of the Big Six

SQL clauses must follow a strict order. Even if logically you think differently, SQL always processes them in this sequence:

    SELECT
    FROM
    WHERE
    GROUP BY
    HAVING
    ORDER BY

Example of a complete query:

    SELECT country, COUNT(*) AS total_customers
    FROM customers
    WHERE active = 1
    GROUP BY country
    HAVING COUNT(*) > 5
    ORDER BY total_customers DESC;

Changing the order will result in a syntax error.

---

## Final Mental Model

Think of SQL queries as a pipeline:

- FROM brings the data
- WHERE removes unwanted rows
- GROUP BY creates buckets
- HAVING removes unwanted buckets
- SELECT decides what to show
- ORDER BY arranges the final output

Mastering the Big Six means mastering SQL itself, because nearly every query you will ever write is built from these same components.
