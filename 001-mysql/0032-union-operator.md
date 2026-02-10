# The UNION Operator in SQL

The `UNION` operator is used to **combine the results of multiple SELECT queries into a single result set**. It allows you to stack results vertically, as long as the queries are compatible.

`UNION` is commonly used when:
- Data comes from similar tables
- Results need to be combined from different conditions
- Logical separation is easier than writing one complex query

---

## What UNION Does

`UNION` combines rows from two or more `SELECT` statements and removes duplicate rows by default.

Key characteristics:
- Result sets are combined vertically
- Duplicate rows are removed
- Column structure must match

---

## Basic UNION Syntax

    SELECT column_list
    FROM table_one

    UNION

    SELECT column_list
    FROM table_two;

Each `SELECT` statement is executed independently, and the results are merged.

---

## Rules for Using UNION

For a `UNION` to work correctly:

1. Each `SELECT` must return the **same number of columns**
2. Corresponding columns must have **compatible data types**
3. Columns are matched by **position**, not by name
4. Column names in the final result come from the **first SELECT**

Violating any of these rules results in an error.

---

## Basic UNION Example

    SELECT customer_id, amount
    FROM payment
    WHERE amount > 5

    UNION

    SELECT customer_id, amount
    FROM payment
    WHERE amount < 1;

This query:
- Retrieves high-value payments
- Retrieves very small payments
- Combines them into one result set
- Removes duplicates automatically

---

## UNION vs UNION ALL

### UNION

    UNION

- Removes duplicate rows
- Performs an implicit DISTINCT operation
- Slightly slower due to deduplication

---

### UNION ALL

    UNION ALL

- Keeps all rows, including duplicates
- Faster than `UNION`
- Useful when duplicates are meaningful or impossible

Example:

    SELECT rating
    FROM film

    UNION ALL

    SELECT rating
    FROM film;

This returns all ratings twice.

---

## UNION with Different Tables

    SELECT first_name, last_name
    FROM customer

    UNION

    SELECT first_name, last_name
    FROM actor;

This combines names from two different tables into a single list.

---

## Using ORDER BY with UNION

`ORDER BY` can only appear **once**, at the very end of the entire UNION query.

Correct usage:

    SELECT customer_id, amount
    FROM payment
    WHERE amount > 5

    UNION

    SELECT customer_id, amount
    FROM payment
    WHERE amount < 1

    ORDER BY amount DESC;

Using `ORDER BY` inside individual SELECT statements is invalid.

---

## Using Aliases in UNION Queries

Column aliases must be defined in the **first SELECT**.

    SELECT customer_id AS id, amount
    FROM payment
    WHERE amount > 5

    UNION

    SELECT customer_id, amount
    FROM payment
    WHERE amount < 1;

The final column name will be `id`.

---

## Common UNION Gotchas

- Mismatched column counts cause errors
- Data type incompatibility causes failures
- ORDER BY placement is restricted
- UNION removes duplicates unless UNION ALL is used
- Performance can degrade on large result sets

---

## When to Use UNION

Use `UNION` when:
- Results should be combined vertically
- Queries share a common structure
- Data logically belongs in the same result set
- Writing one query would be less readable

Avoid `UNION` when:
- Tables should be joined horizontally
- Relationships exist between rows

---

## Mental Model

Think of `UNION` as stacking tables:

- First result set on top
- Second result set below it
- Duplicates removed unless explicitly allowed

`UNION` is a powerful tool for combining query results while keeping queries simple, readable, and logically separated.
