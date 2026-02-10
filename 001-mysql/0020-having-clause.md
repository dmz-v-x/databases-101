# The HAVING Clause in SQL

The `HAVING` clause is used to **filter grouped data**. While the `WHERE` clause filters individual rows, `HAVING` filters **groups created by `GROUP BY`**. It is specifically designed to work with aggregate functions.

---

## Why HAVING Exists

After data is grouped using `GROUP BY`, SQL no longer works with individual rows. At that stage:
- `WHERE` can no longer be applied
- Aggregate values like `COUNT`, `SUM`, or `AVG` now exist

The `HAVING` clause allows filtering **after aggregation** has occurred.

---

## Basic Syntax of HAVING

    HAVING logical_condition

A complete query structure looks like this:

    SELECT column_name, aggregate_function(column_name)
    FROM table_name
    WHERE row_level_condition
    GROUP BY column_name
    HAVING group_level_condition
    ORDER BY column_name;

---

## HAVING Must Be Used with GROUP BY

The `HAVING` clause:
- Can only be used when `GROUP BY` is present
- Always comes **after `GROUP BY`**
- Always comes **before `ORDER BY`**

Using `HAVING` without `GROUP BY` results in an error in most SQL systems.

---

## Basic HAVING Example

    SELECT rating, COUNT(film_id)
    FROM film
    GROUP BY rating
    HAVING COUNT(film_id) > 50;

This query:
- Groups films by rating
- Counts films in each rating group
- Returns only ratings with more than 50 films

---

## HAVING vs WHERE (Critical Difference)

### WHERE Filters Rows

    SELECT rating, COUNT(film_id)
    FROM film
    WHERE rental_duration > 3
    GROUP BY rating;

- Filters rows **before grouping**
- Only films with rental duration greater than 3 are grouped

---

### HAVING Filters Groups

    SELECT rating, COUNT(film_id)
    FROM film
    GROUP BY rating
    HAVING COUNT(film_id) > 20;

- Groups all rows first
- Filters groups **after aggregation**

---

## Using WHERE and HAVING Together

`WHERE` and `HAVING` often appear together, each serving a different purpose.

    SELECT customer_id, SUM(amount) AS total_spent
    FROM payment
    WHERE payment_date > '2006-01-01'
    GROUP BY customer_id
    HAVING SUM(amount) > 100;

Query flow:
- `WHERE` removes old payment records
- `GROUP BY` groups payments per customer
- `HAVING` keeps only customers who spent more than 100

---

## Using HAVING with Multiple Conditions

    SELECT rating, AVG(rental_duration)
    FROM film
    GROUP BY rating
    HAVING AVG(rental_duration) > 4
       AND COUNT(film_id) > 30;

This filters groups based on:
- Average rental duration
- Number of films in the group

---

## HAVING with Aliases

In many SQL dialects, aliases created in `SELECT` can be used in `HAVING`.

    SELECT rating, COUNT(film_id) AS film_count
    FROM film
    GROUP BY rating
    HAVING film_count > 40;

This improves readability, though support depends on the database system.

---

## Common HAVING Gotchas

- `HAVING` cannot replace `WHERE`
- `WHERE` cannot use aggregate functions
- `HAVING` operates only on grouped results
- Using `HAVING` without `GROUP BY` is usually invalid
- `HAVING` can significantly impact performance on large datasets

---

## Mental Model for HAVING

Think of SQL processing in layers:

- `WHERE` filters **rows**
- `GROUP BY` creates **groups**
- `HAVING` filters **groups**

If `WHERE` is a sieve for rows, `HAVING` is a sieve for summaries.

Understanding the `HAVING` clause is essential for writing correct analytical queries and working confidently with aggregated data.
