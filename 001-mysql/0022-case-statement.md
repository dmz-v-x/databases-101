# The CASE Statement in SQL

The `CASE` statement allows conditional logic inside SQL queries. It enables you to evaluate conditions and return different values based on which condition is met. Conceptually, it is similar to `if / else` logic in programming languages.

`CASE` is commonly used to:
- Create derived columns
- Categorize data
- Apply conditional transformations
- Improve readability of query results

---

## What the CASE Statement Does

The `CASE` statement evaluates conditions **row by row** and returns a value when a condition is satisfied.

Basic structure:

    CASE
        WHEN condition THEN result
        WHEN condition THEN result
        ELSE result
    END

The first condition that evaluates to true determines the output.

---

## Simple CASE Example

    SELECT
        title,
        rating,
        CASE
            WHEN rating = 'G' THEN 'General Audience'
            WHEN rating = 'PG' THEN 'Parental Guidance'
            WHEN rating = 'R' THEN 'Restricted'
            ELSE 'Other'
        END AS rating_description
    FROM film;

This query:
- Checks the `rating` value for each film
- Maps ratings to descriptive labels
- Returns a new derived column called `rating_description`

---

## CASE with Numeric Conditions

    SELECT
        payment_id,
        amount,
        CASE
            WHEN amount < 2 THEN 'Low'
            WHEN amount BETWEEN 2 AND 5 THEN 'Medium'
            WHEN amount > 5 THEN 'High'
        END AS payment_category
    FROM payment;

Each payment is categorized based on its amount.

---

## CASE with GROUP BY and Aggregates

`CASE` can be used inside aggregate functions to perform conditional aggregation.

    SELECT
        rating,
        COUNT(
            CASE
                WHEN rental_duration > 5 THEN film_id
            END
        ) AS long_rentals
    FROM film
    GROUP BY rating;

This counts only films with a rental duration greater than 5, grouped by rating.

---

## CASE in ORDER BY

`CASE` can be used to define custom sorting logic.

    SELECT
        title,
        rating
    FROM film
    ORDER BY
        CASE
            WHEN rating = 'G' THEN 1
            WHEN rating = 'PG' THEN 2
            WHEN rating = 'R' THEN 3
            ELSE 4
        END;

This enforces a custom rating order instead of alphabetical sorting.

---

## CASE in WHERE Clause

`CASE` can be used indirectly in filtering logic, though clarity is important.

    SELECT *
    FROM payment
    WHERE
        CASE
            WHEN amount > 5 THEN 1
            ELSE 0
        END = 1;

This filters rows where the amount is greater than 5.

---

## Searched CASE vs Simple CASE

### Searched CASE (Most Common)

    CASE
        WHEN condition THEN result
        WHEN condition THEN result
        ELSE result
    END

Conditions can be any logical expression.

---

### Simple CASE

    CASE column_name
        WHEN value THEN result
        WHEN value THEN result
        ELSE result
    END

Example:

    SELECT
        rating,
        CASE rating
            WHEN 'G' THEN 'Kids'
            WHEN 'PG' THEN 'Family'
            WHEN 'R' THEN 'Adults'
            ELSE 'Other'
        END AS audience_type
    FROM film;

This compares one column against multiple values.

---

## Important CASE Rules and Gotchas

- `CASE` always returns a single value per row
- The `ELSE` clause is optional but recommended
- If no condition matches and no `ELSE` is provided, the result is `NULL`
- `CASE` does not stop query execution; it only affects output values
- Data types returned by `THEN` and `ELSE` should be compatible

---

## Mental Model for CASE

Think of `CASE` as a decision tree inside your query:

- Each row is evaluated independently
- Conditions are checked top to bottom
- The first matching condition wins
- A final fallback is provided by `ELSE`

The `CASE` statement is a powerful tool for transforming raw data into meaningful, human-readable results directly within SQL queries.
