# SELECT DISTINCT in SQL

In many databases, tables contain repeated values across rows. When the goal is to understand **which values exist**, rather than how many times they appear, SQL provides the `SELECT DISTINCT` keyword. This allows result sets to return only unique values.

---

## What SELECT DISTINCT Does

`SELECT DISTINCT` removes duplicate rows from the result set.

Basic syntax:

    SELECT DISTINCT column_name
    FROM table_name;

This instructs SQL to:
- Read the specified column
- Eliminate duplicate values
- Return only unique results

---

## Selecting Distinct Values from a Single Column

A common use of `SELECT DISTINCT` is to explore all possible values in a column.

Example:

    SELECT DISTINCT rating
    FROM film;

If many films share the same rating, each rating will appear only once in the result set.

This is especially useful for:
- Understanding categorical data
- Exploring enums or status fields
- Validating allowed values in a column

---

## Selecting Distinct Values Across Multiple Columns

When multiple columns are listed, SQL returns **distinct combinations**, not distinct values per column.

Syntax:

    SELECT DISTINCT column_one, column_two
    FROM table_name;

Example:

    SELECT DISTINCT first_name, last_name
    FROM actor;

This returns:
- Unique combinations of `first_name` and `last_name`
- Duplicate pairs are removed
- Individual column values may still repeat

---

## Important Behavior to Understand

Distinctness applies to the **entire row**, not each column independently.

Example:

    SELECT DISTINCT city, country
    FROM address;

If the same city appears in different countries, each combination is considered unique and is included.

---

## SELECT DISTINCT vs SELECT

Without `DISTINCT`:

    SELECT rating
    FROM film;

Duplicate values appear multiple times.

With `DISTINCT`:

    SELECT DISTINCT rating
    FROM film;

Each value appears only once.

---

## Common Gotchas with SELECT DISTINCT

- `DISTINCT` applies to all selected columns together
- It does not guarantee any specific order of results
- Ordering requires an explicit `ORDER BY`
- Using `DISTINCT` on many columns can be expensive on large tables

---

## When to Use SELECT DISTINCT

`SELECT DISTINCT` is ideal when:
- Exploring a dataset
- Identifying valid categories
- Understanding data variety
- Removing duplicates from result sets

It is not meant to replace grouping or aggregation, but it is a powerful tool for quickly understanding the structure of your data.

---

## Mental Model for SELECT DISTINCT

Think of `SELECT DISTINCT` as asking:

“Show me each unique version of this data.”

It collapses repeated rows into a single representation, making patterns and categories easier to see.
