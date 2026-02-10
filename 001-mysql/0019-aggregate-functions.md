# Aggregate Functions in SQL

Aggregate functions are used to **summarize data**. Instead of returning individual rows, they perform calculations on a group of rows and return a **single value per group**. They are most commonly used with the `GROUP BY` clause but can also be used on an entire table.

Aggregate functions are essential for analysis, reporting, and understanding patterns in data.

---

## What Aggregate Functions Do

Aggregate functions take multiple rows as input and return **one summarized result**.

They are commonly used to answer questions like:
- How many records exist?
- What is the total value?
- What is the average?
- What is the minimum or maximum value?

---

## Common Aggregate Functions

### COUNT()

`COUNT()` returns the number of rows.

Example: counting all rows in a table

    SELECT COUNT(*)
    FROM film;

This returns the total number of films.

Example: counting non-null values in a column

    SELECT COUNT(return_date)
    FROM rental;

Only rows where `return_date` is not NULL are counted.

---

### SUM()

`SUM()` returns the total of a numeric column.

Example:

    SELECT SUM(amount)
    FROM payment;

This returns the total amount of all payments.

Example with grouping:

    SELECT customer_id, SUM(amount) AS total_spent
    FROM payment
    GROUP BY customer_id;

This returns the total amount spent by each customer.

---

### AVG()

`AVG()` returns the average value of a numeric column.

Example:

    SELECT AVG(amount)
    FROM payment;

This calculates the average payment amount.

Example with grouping:

    SELECT rating, AVG(rental_duration)
    FROM film
    GROUP BY rating;

This returns the average rental duration per rating.

---

### MIN()

`MIN()` returns the smallest value in a column.

Example:

    SELECT MIN(amount)
    FROM payment;

This returns the lowest payment amount.

Example with grouping:

    SELECT customer_id, MIN(amount)
    FROM payment
    GROUP BY customer_id;

This returns the smallest payment made by each customer.

---

### MAX()

`MAX()` returns the largest value in a column.

Example:

    SELECT MAX(amount)
    FROM payment;

This returns the highest payment amount.

Example with grouping:

    SELECT rating, MAX(rental_duration)
    FROM film
    GROUP BY rating;

This returns the maximum rental duration per rating.

---

## Using Aggregate Functions with GROUP BY

When aggregate functions are used alongside non-aggregated columns, `GROUP BY` is required.

Example:

    SELECT rating, COUNT(film_id)
    FROM film
    GROUP BY rating;

Rules to remember:
- Every column in `SELECT` must either be aggregated or listed in `GROUP BY`
- Aggregate functions operate **per group**, not per row

---

## Aggregate Functions Without GROUP BY

If no `GROUP BY` clause is used:
- The entire table is treated as a single group
- Only one row is returned

Example:

    SELECT COUNT(*), SUM(amount), AVG(amount)
    FROM payment;

This produces a single summary row.

---

## Aggregate Functions and WHERE vs HAVING

- `WHERE` filters rows **before** aggregation
- `HAVING` filters groups **after** aggregation

Example:

    SELECT customer_id, SUM(amount)
    FROM payment
    WHERE payment_date > '2006-01-01'
    GROUP BY customer_id
    HAVING SUM(amount) > 50;

---

## Common Gotchas with Aggregate Functions

- Aggregate functions ignore NULL values (except `COUNT(*)`)
- You cannot use aggregate functions directly in `WHERE`
- Aliases created in `SELECT` cannot be used in `WHERE`
- Aggregates change the result from row-level to group-level

---

## Mental Model

Think of aggregate functions as **summarizers**:

- Rows go in
- A single value comes out
- `GROUP BY` decides how many summaries you get

Mastering aggregate functions is critical for turning raw data into meaningful insights in SQL.
