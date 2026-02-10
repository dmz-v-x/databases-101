# FULL OUTER JOIN in SQL

A `FULL OUTER JOIN` is used to combine data from two tables while **preserving all rows from both tables**. Rows that match between the tables are combined into a single row, and rows that do not match appear with `NULL` values for the missing side.

This join type is useful when you want a **complete view of all data**, regardless of whether relationships exist.

---

## What FULL OUTER JOIN Does

A `FULL OUTER JOIN` returns:
- All matching rows from both tables
- All non-matching rows from the left table
- All non-matching rows from the right table
- `NULL` values where a match does not exist

No data from either table is lost.

---

## Basic FULL OUTER JOIN Syntax

    SELECT column_list
    FROM table_one
    FULL OUTER JOIN table_two
      ON join_condition;

The `ON` clause defines how rows from the two tables are related.

---

## Example: Customers and Payments

    SELECT
        c.customer_id,
        c.first_name,
        p.payment_id,
        p.amount
    FROM customer c
    FULL OUTER JOIN payment p
      ON c.customer_id = p.customer_id;

This query returns:
- Customers who have payments
- Customers who have no payments
- Payments that are not linked to any customer

Rows without matches contain `NULL` values for the missing table’s columns.

---

## Understanding the Result Set

A `FULL OUTER JOIN` result includes three categories of rows:

1. Rows that match in both tables  
2. Rows that exist only in the left table  
3. Rows that exist only in the right table  

This makes it ideal for data reconciliation and auditing.

---

## FULL OUTER JOIN with Filtering

Filtering must be done carefully to avoid unintentionally removing rows.

Example:

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    FULL OUTER JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE c.customer_id IS NULL
       OR p.customer_id IS NULL;

This returns only rows where a match does **not** exist on one side.

---

## Database Support Considerations

Not all databases support `FULL OUTER JOIN`.

- PostgreSQL: Supported
- SQL Server: Supported
- Oracle: Supported
- MySQL: Not supported directly

In MySQL, a `FULL OUTER JOIN` must be simulated using a combination of `LEFT JOIN` and `RIGHT JOIN`.

---

## Simulating FULL OUTER JOIN in MySQL

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id

    UNION

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id;

This produces the same logical result as a `FULL OUTER JOIN`.

---

## FULL OUTER JOIN vs Other JOINs

- INNER JOIN: Only matching rows
- LEFT JOIN: All left rows + matching right rows
- RIGHT JOIN: All right rows + matching left rows
- FULL OUTER JOIN: All rows from both tables

`FULL OUTER JOIN` is the most inclusive join type.

---

## Common FULL OUTER JOIN Gotchas

- Filtering in `WHERE` can remove unmatched rows
- Large result sets can impact performance
- NULL handling is critical
- Not supported in all SQL dialects

---

## Mental Model

Think of `FULL OUTER JOIN` as saying:

“Give me everything from both tables, and line things up where they match.”

It is the best choice when completeness matters more than strict relational matching.
