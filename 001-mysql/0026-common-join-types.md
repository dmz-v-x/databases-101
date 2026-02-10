# Common JOIN Types in SQL

In relational databases, data is spread across multiple tables. To combine related data from these tables into a single result set, SQL uses **JOINs**. Different JOIN types control **which rows are included** in the final output based on how records match between tables.

Understanding JOIN types is critical for writing correct multi-table queries.

---

## INNER JOIN

### What INNER JOIN Does

`INNER JOIN` returns **only the rows that have matching values in both tables**.

If a row does not have a match in either table, it is excluded from the result.

### Syntax

    SELECT columns
    FROM table_a
    INNER JOIN table_b
      ON join_condition;

### Example

    SELECT
        c.customer_id,
        c.first_name,
        p.amount
    FROM customer c
    INNER JOIN payment p
      ON c.customer_id = p.customer_id;

This returns:
- Customers who have made payments
- Only rows where a customer and payment match

Customers without payments are excluded.

---

## LEFT JOIN (LEFT OUTER JOIN)

### What LEFT JOIN Does

`LEFT JOIN` returns:
- **All rows from the left table**
- Matching rows from the right table
- `NULL` values where no match exists

### Syntax

    SELECT columns
    FROM table_a
    LEFT JOIN table_b
      ON join_condition;

### Example

    SELECT
        c.customer_id,
        c.first_name,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id;

This returns:
- All customers
- Payment amounts where available
- `NULL` for customers with no payments

LEFT JOIN is useful for finding missing or optional relationships.

---

## RIGHT JOIN (RIGHT OUTER JOIN)

### What RIGHT JOIN Does

`RIGHT JOIN` returns:
- **All rows from the right table**
- Matching rows from the left table
- `NULL` values where no match exists

### Syntax

    SELECT columns
    FROM table_a
    RIGHT JOIN table_b
      ON join_condition;

### Example

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id;

This returns:
- All payments
- Customer data where available

In practice, `RIGHT JOIN` is rarely used because the same result can usually be achieved with a `LEFT JOIN` by reversing table order.

---

## FULL JOIN (FULL OUTER JOIN)

### What FULL JOIN Does

`FULL JOIN` returns:
- All rows from both tables
- Matches where they exist
- `NULL` values where no match exists on either side

### Syntax

    SELECT columns
    FROM table_a
    FULL JOIN table_b
      ON join_condition;

### Example

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    FULL JOIN payment p
      ON c.customer_id = p.customer_id;

This returns:
- Customers with payments
- Customers without payments
- Payments without customers

Note: Some databases (including MySQL) do not support `FULL JOIN` directly and require workarounds.

---

## CROSS JOIN

### What CROSS JOIN Does

`CROSS JOIN` returns the **Cartesian product** of two tables:
- Every row from the first table is paired with every row from the second table

### Syntax

    SELECT columns
    FROM table_a
    CROSS JOIN table_b;

### Example

    SELECT
        c.customer_id,
        s.store_id
    FROM customer c
    CROSS JOIN store s;

If there are:
- 100 customers
- 2 stores

The result contains 200 rows.

CROSS JOIN is rarely used intentionally and can easily create extremely large result sets.

---

## SELF JOIN

### What SELF JOIN Does

A `SELF JOIN` joins a table to itself. It is used when rows in the same table relate to each other.

### Syntax

    SELECT columns
    FROM table a
    JOIN table b
      ON condition;

### Example

    SELECT
        e.employee_id,
        e.name,
        m.name AS manager_name
    FROM employee e
    JOIN employee m
      ON e.manager_id = m.employee_id;

This relates employees to their managers using the same table.

---

## JOIN Type Comparison Summary

- `INNER JOIN`: Only matching rows from both tables
- `LEFT JOIN`: All rows from left table, optional matches from right
- `RIGHT JOIN`: All rows from right table, optional matches from left
- `FULL JOIN`: All rows from both tables
- `CROSS JOIN`: Every possible row combination
- `SELF JOIN`: Table joined to itself

---

## Common JOIN Gotchas

- Missing join conditions cause Cartesian products
- Filtering in `WHERE` can accidentally turn outer joins into inner joins
- Ambiguous column names require table prefixes
- Incorrect join keys lead to incorrect results
- Large joins can impact performance significantly

---

## Mental Model

Think of JOINs as rules for combining tables:

- INNER JOIN keeps only intersections
- LEFT JOIN keeps everything on the left
- RIGHT JOIN keeps everything on the right
- FULL JOIN keeps everything from both sides
- CROSS JOIN keeps all combinations

Choosing the correct JOIN type determines **which data survives** in your final result set.
