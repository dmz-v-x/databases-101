# The WHERE Clause in SQL

The `WHERE` clause is used to **filter rows** in a query result set. It allows you to specify conditions that determine which records should be included and which should be excluded from the results.

Unlike `SELECT`, which controls columns, `WHERE` controls **which rows survive**.

---

## What the WHERE Clause Does

The `WHERE` clause applies a logical condition to each row in the table and keeps only those rows that satisfy the condition.

Basic syntax:

    WHERE logical_condition

Example:

    SELECT *
    FROM film
    WHERE category = 'Sci-Fi';

Only rows that meet the condition are returned.

---

## WHERE Is Optional but Powerful

The `WHERE` clause is optional. If it is omitted, SQL returns **all rows** from the table.

Example without WHERE:

    SELECT *
    FROM payment;

This returns every row in the table.

Adding a WHERE clause narrows the result set to only relevant records.

---

## Common WHERE Conditions

### Equality Conditions

    WHERE category = 'Sci-Fi'

Returns rows where the category is exactly `Sci-Fi`.

---

### Comparison Conditions

    WHERE amount > 5.99

Returns rows where the value in the `amount` column is greater than 5.99.

Other common comparison operators include:
- `<` less than
- `<=` less than or equal to
- `>=` greater than or equal to
- `<>` not equal

---

### Date Range Filtering

    WHERE rental_date BETWEEN '2006-01-01' AND '2006-06-01'

Returns rows where `rental_date` falls within the specified date range, inclusive.

This is commonly used when filtering by time periods.

---

## WHERE Clause Placement in a Query

The `WHERE` clause always appears:
- After the `FROM` clause
- Before `GROUP BY`, `HAVING`, and `ORDER BY` (if present)

Correct structure:

    SELECT column_name
    FROM table_name
    WHERE logical_condition
    GROUP BY column_name
    HAVING logical_condition
    ORDER BY column_name;

Changing this order results in a syntax error.

---

## Important WHERE Gotchas

- `WHERE` filters rows, not columns
- Aggregate functions cannot be used in `WHERE`
- Conditions are evaluated before grouping occurs
- `NULL` values require `IS NULL` or `IS NOT NULL`, not `=`

---

## Mental Model for WHERE

Think of `WHERE` as a gatekeeper.

- Each row is checked against the condition
- Rows that pass go through
- Rows that fail are discarded

Understanding the `WHERE` clause is essential, as it is one of the most frequently used tools for controlling query results in SQL.
