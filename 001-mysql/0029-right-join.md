# RIGHT JOIN in SQL

A `RIGHT JOIN` (also called a `RIGHT OUTER JOIN`) is used to combine data from two tables while **preserving all rows from the right table**, even when there is no matching row in the left table. When no match exists, SQL fills the left-side columns with `NULL`.

---

## What RIGHT JOIN Does

`RIGHT JOIN` returns:
- All rows from the **right table**
- Matching rows from the **left table**
- `NULL` values for left-table columns when no match exists

This makes `RIGHT JOIN` useful when the table on the right must be fully preserved.

---

## Basic RIGHT JOIN Syntax

    SELECT column_list
    FROM table_one
    RIGHT JOIN table_two
      ON join_condition;

The table listed after `RIGHT JOIN` is the **right table** and determines which rows are always included.

---

## Example: Customers and Payments

    SELECT
        c.customer_id,
        c.first_name,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id;

This query:
- Returns all payments
- Includes customer information where it exists
- Shows `NULL` for customer columns if a payment is not linked to a customer

---

## RIGHT JOIN with Filtering

### Correct Way to Filter Left Table Data

To preserve unmatched rows from the right table, conditions on the left table must be placed in the `ON` clause.

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id
     AND c.active = 1;

This keeps:
- All payments
- Only active customers where they exist
- Payments without an active customer still appear with `NULL`

---

### Common Mistake: Filtering in WHERE

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE c.active = 1;

This effectively turns the query into an **INNER JOIN**, because:
- Rows with `NULL` customer values are filtered out
- Payments without matching customers are removed

---

## Finding Missing Matches with RIGHT JOIN

    SELECT
        p.payment_id,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE c.customer_id IS NULL;

This returns payments that are **not linked to any customer**.

---

## RIGHT JOIN vs LEFT JOIN

Any `RIGHT JOIN` can be rewritten as a `LEFT JOIN` by swapping table order.

Equivalent queries:

    SELECT *
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id;

is the same as:

    SELECT *
    FROM payment p
    LEFT JOIN customer c
      ON p.customer_id = c.customer_id;

Because of this, many developers prefer `LEFT JOIN` for consistency.

---

## Important RIGHT JOIN Characteristics

- The right table defines the preserved row set
- Left-table columns may contain `NULL`
- Table order matters
- Less commonly used than `LEFT JOIN`

---

## Common RIGHT JOIN Gotchas

- Filtering in `WHERE` can unintentionally remove rows
- Misplaced conditions change join behavior
- Can reduce readability in complex queries
- Often avoidable using `LEFT JOIN`

---

## Mental Model

Think of `RIGHT JOIN` as saying:

“Give me everything from the right table, and include related data from the left table if it exists.”

While fully valid SQL, `RIGHT JOIN` is most useful when reading queries where the right table is logically the primary dataset.
