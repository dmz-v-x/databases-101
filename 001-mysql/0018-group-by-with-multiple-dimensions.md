# GROUP BY with Multiple Dimensions in SQL

The `GROUP BY` clause is not limited to a single column. SQL allows grouping data across **multiple dimensions** by listing more than one column in the `GROUP BY` clause. This enables more granular summaries and deeper analysis.

---

## What Multiple-Dimension GROUP BY Means

When multiple columns are listed in `GROUP BY`, SQL creates groups based on **unique combinations** of those columns.

Basic syntax:

    GROUP BY column_one, column_two

Each distinct pairing of values across the listed columns forms its own group.

---

## Example: Grouping by Rating and Rental Duration

    SELECT
        rating,
        rental_duration,
        COUNT(film_id) AS count_of_films
    FROM film
    GROUP BY rating, rental_duration;

This query:
- Groups films by `rating`
- Further divides each rating by `rental_duration`
- Counts how many films exist for each unique combination

The result set contains:
- One row per `(rating, rental_duration)` pair
- A count representing how many films fall into that specific category

---

## How SQL Interprets This Grouping

Conceptually, SQL performs the following steps:
- Reads all rows from the `film` table
- Creates buckets for each unique combination of:
  - `rating`
  - `rental_duration`
- Counts how many rows fall into each bucket
- Returns one summarized row per bucket

---

## Why Use Multiple GROUP BY Columns

Multi-column grouping is useful when:
- A single column summary is too broad
- You need breakdowns across multiple attributes
- You want to compare distributions within categories

Examples of real-world use cases:
- Sales totals by region and month
- User counts by country and device type
- Product sales by category and price range

---

## Important Rules to Remember

- Every non-aggregated column in `SELECT` must appear in `GROUP BY`
- The order of columns in `GROUP BY` defines the grouping hierarchy
- The number of result rows equals the number of unique combinations
- Adding more columns increases result granularity

---

## Common Gotchas

- Forgetting to include a selected column in `GROUP BY` causes errors
- Grouping by many columns can significantly increase result size
- `WHERE` filters rows before grouping; `HAVING` filters groups after grouping
- Column aliases cannot be used inside the `GROUP BY` clause

---

## Mental Model

Think of multi-column `GROUP BY` as creating **multi-level buckets**:

- First level groups by the first column
- Second level subdivides those groups by the next column
- Aggregate functions summarize each final bucket

Using multiple dimensions with `GROUP BY` is a powerful way to analyze how data behaves across different attributes simultaneously.
