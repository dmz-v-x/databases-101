# INNER JOIN in SQL

An `INNER JOIN` is the most commonly used join type in SQL. It is used to combine rows from two or more tables **only when there is a matching value in all joined tables**. Rows that do not satisfy the join condition are excluded from the result set.

---

## What INNER JOIN Does

`INNER JOIN` returns the **intersection** of two tables.

- Rows must exist in **both tables**
- The join condition must evaluate to true
- Non-matching rows are discarded

If there is no match, the row does not appear in the results.

---

## Basic INNER JOIN Syntax

    SELECT column_list
    FROM table_one
    INNER JOIN table_two
      ON join_condition;

The `ON` clause defines how rows from the two tables are related, usually through primary and foreign keys.

---

## Example: Joining Customers and Payments

    SELECT
        c.customer_id,
        c.first_name,
        c.last_name,
        p.amount,
        p.payment_date
    FROM customer c
    INNER JOIN payment p
      ON c.customer_id = p.customer_id;

This query:
- Matches customers to their payments
- Returns only customers who have made payments
- Excludes customers with no payments
- Excludes payments that do not match a customer

---

## INNER JOIN with Filtering

`INNER JOIN` is often combined with `WHERE` to further restrict results.

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    INNER JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE p.amount > 5;

This returns:
- Only matching customer–payment pairs
- Only payments greater than 5

Filtering happens **after** the join condition is applied.

---

## INNER JOIN with Multiple Tables

`INNER JOIN` can be chained to join more than two tables.

    SELECT
        c.customer_id,
        f.title,
        p.amount
    FROM customer c
    INNER JOIN rental r
      ON c.customer_id = r.customer_id
    INNER JOIN inventory i
      ON r.inventory_id = i.inventory_id
    INNER JOIN film f
      ON i.film_id = f.film_id
    INNER JOIN payment p
      ON r.rental_id = p.rental_id;

Each join adds another requirement for a match. A row must satisfy **all join conditions** to appear in the final result.

---

## INNER JOIN vs WHERE Join Syntax

Older SQL syntax allowed joins using `WHERE`:

    SELECT *
    FROM customer c, payment p
    WHERE c.customer_id = p.customer_id;

This is functionally similar but:
- Harder to read
- Easier to make mistakes
- Not recommended for modern SQL

Explicit `INNER JOIN ... ON` syntax is clearer and safer.

---

## Important INNER JOIN Characteristics

- Returns fewer rows than either table alone
- Excludes NULL matches automatically
- Order of tables does not affect the result
- Requires an explicit join condition

---

## Common INNER JOIN Gotchas

- Missing `ON` condition results in a Cartesian product
- Joining on the wrong columns produces incorrect data
- Ambiguous column names require table prefixes or aliases
- INNER JOIN silently removes unmatched rows

---

## Mental Model

Think of `INNER JOIN` as an overlap operation:

- Only rows that exist in **both tables**
- Only rows that satisfy the relationship
- Everything else is ignored

If a row does not belong to both tables, it does not belong in the result. This makes `INNER JOIN` ideal when you only want **confirmed, fully-related data**.
