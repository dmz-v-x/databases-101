# LEFT JOIN in SQL

A `LEFT JOIN` (also called a `LEFT OUTER JOIN`) is used to combine data from two tables while **preserving all rows from the left table**, even when there is no matching row in the right table. When no match exists, SQL fills the right-side columns with `NULL`.

---

## What LEFT JOIN Does

`LEFT JOIN` returns:
- All rows from the **left table**
- Matching rows from the **right table**
- `NULL` values for right-table columns when no match exists

This makes `LEFT JOIN` ideal for identifying missing or optional relationships.

---

## Basic LEFT JOIN Syntax

    SELECT column_list
    FROM table_one
    LEFT JOIN table_two
      ON join_condition;

The table listed before `LEFT JOIN` is the **left table** and determines which rows are always kept.

---

## Example: Customers and Payments

    SELECT
        c.customer_id,
        c.first_name,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id;

This query:
- Returns all customers
- Includes payment amounts where they exist
- Shows `NULL` for customers who have never made a payment

Customers are never removed from the result set.

---

## LEFT JOIN with Filtering

### Correct Way to Filter Right Table Data

To preserve unmatched rows, conditions on the right table must be placed in the `ON` clause.

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id
     AND p.amount > 5;

This keeps:
- All customers
- Only payments greater than 5
- Customers with no qualifying payments still appear with `NULL`

---

### Common Mistake: Filtering in WHERE

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE p.amount > 5;

This converts the `LEFT JOIN` into an **INNER JOIN** because:
- Rows with `NULL` values are filtered out
- Customers without payments disappear

---

## Finding Missing Data with LEFT JOIN

A common pattern is using `LEFT JOIN` with a `NULL` check.

    SELECT
        c.customer_id,
        c.first_name
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE p.customer_id IS NULL;

This returns customers who have **never made a payment**.

---

## LEFT JOIN with Multiple Tables

    SELECT
        c.customer_id,
        f.title
    FROM customer c
    LEFT JOIN rental r
      ON c.customer_id = r.customer_id
    LEFT JOIN inventory i
      ON r.inventory_id = i.inventory_id
    LEFT JOIN film f
      ON i.film_id = f.film_id;

Each `LEFT JOIN` preserves all rows from the left side of that join chain.

---

## Important LEFT JOIN Characteristics

- The left table always determines the base row set
- Right-table columns may contain `NULL`
- Order of tables matters
- Useful for optional or missing relationships

---

## Common LEFT JOIN Gotchas

- Filtering in `WHERE` can break the join
- Misplacing conditions changes query meaning
- `NULL` values must be handled explicitly
- Large left joins can impact performance

---

## Mental Model

Think of `LEFT JOIN` as saying:

“Give me everything from the left table, and include related data from the right table if it exists.”

Rows on the left are never lost, making `LEFT JOIN` essential for completeness and data auditing.
