# Using COUNT with CASE in SQL

`COUNT` and `CASE` are often used together to perform **conditional counting**. This technique allows you to count rows **only when certain conditions are met**, all within a single query. It is a powerful pattern for analytics and reporting.

---

## Why Combine COUNT and CASE

Normally, `COUNT()` counts rows without conditions.

Example:

    SELECT COUNT(*)
    FROM payment;

This counts **all rows**.

But in real-world analysis, questions are usually conditional:
- How many payments were above a certain amount?
- How many rentals were returned late?
- How many customers fall into a specific category?

This is where `CASE` inside `COUNT` becomes useful.

---

## Basic Pattern: COUNT with CASE

The most common pattern looks like this:

    COUNT(
        CASE
            WHEN condition THEN column_name
        END
    )

Key behavior:
- `CASE` returns a value only when the condition is true
- `COUNT` ignores `NULL` values
- Rows that do not match the condition are not counted

---

## Example 1: Counting Conditional Rows

    SELECT
        COUNT(
            CASE
                WHEN amount > 5 THEN payment_id
            END
        ) AS high_value_payments
    FROM payment;

This query:
- Checks each payment
- Counts only payments where `amount > 5`
- Ignores all other rows

---

## Example 2: Multiple Conditional Counts in One Query

    SELECT
        COUNT(
            CASE
                WHEN amount < 2 THEN payment_id
            END
        ) AS low_payments,
        COUNT(
            CASE
                WHEN amount BETWEEN 2 AND 5 THEN payment_id
            END
        ) AS medium_payments,
        COUNT(
            CASE
                WHEN amount > 5 THEN payment_id
            END
        ) AS high_payments
    FROM payment;

This produces **multiple conditional counts** from the same table in a single pass.

---

## Example 3: COUNT + CASE with GROUP BY

    SELECT
        customer_id,
        COUNT(
            CASE
                WHEN amount > 5 THEN payment_id
            END
        ) AS high_value_payments
    FROM payment
    GROUP BY customer_id;

This query:
- Groups payments by customer
- Counts only high-value payments per customer
- Returns one row per customer

---

## Example 4: Counting Categories Using CASE Labels

    SELECT
        rating,
        COUNT(
            CASE
                WHEN rental_duration > 5 THEN film_id
            END
        ) AS long_rentals
    FROM film
    GROUP BY rating;

This counts how many films in each rating category have a rental duration greater than 5.

---

## COUNT(CASE ...) vs COUNT(*)

Important distinction:

- `COUNT(*)` counts all rows
- `COUNT(column)` counts non-null values
- `COUNT(CASE WHEN ... THEN column END)` counts rows matching the condition

Using `CASE` gives precise control over what gets counted.

---

## Alternative: SUM with CASE

Sometimes the same logic is written using `SUM`:

    SELECT
        SUM(
            CASE
                WHEN amount > 5 THEN 1
                ELSE 0
            END
        ) AS high_value_payments
    FROM payment;

Both approaches are valid:
- `COUNT(CASE ...)` relies on NULL handling
- `SUM(CASE ...)` relies on numeric addition

---

## Common Gotchas

- `COUNT` ignores `NULL` values
- The column inside `COUNT` must not always be NULL
- Conditions in `CASE` are evaluated row by row
- Forgetting `GROUP BY` changes the level of aggregation
- Aliases cannot be used inside the same `SELECT` clause

---

## Mental Model

Think of `CASE` as a filter inside the aggregation:

- Each row is checked
- Matching rows return a value
- Non-matching rows return `NULL`
- `COUNT` counts only the rows that returned a value

Combining `COUNT` and `CASE` allows SQL queries to answer complex analytical questions efficiently and clearly.
