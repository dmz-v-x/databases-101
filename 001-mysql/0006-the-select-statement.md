# The SELECT Statement in SQL

The `SELECT` statement is the foundation of SQL. It defines **what data you want to retrieve** from a database and determines the shape of the result set returned by the SQL server. Every meaningful SQL query includes a `SELECT` clause, paired with a `FROM` clause.

---

## What SELECT Does

At its core, `SELECT` identifies **which columns** should appear in the query results.

Basic syntax:

    SELECT column_name
    FROM table_name;

This tells the SQL server:
- Look at the specified table
- Retrieve **all rows**
- Return only the column(s) listed in the `SELECT` clause

---

## Selecting Multiple Columns

You can request more than one column by separating column names with commas.

Syntax:

    SELECT column_name, other_column_name
    FROM table_name;

Example:

    SELECT first_name, last_name, email
    FROM customers;

This returns:
- Every row in the `customers` table
- Only the `first_name`, `last_name`, and `email` columns
- Columns appear in the result in the same order they are listed

---

## SELECT Is Required in Every Query

Every SQL query must include:
- `SELECT` — what data you want
- `FROM` — where the data comes from

Minimal valid query structure:

    SELECT column_name
    FROM table_name;

Without `SELECT`, SQL does not know what to return.  
Without `FROM`, SQL does not know where the data lives.

---

## SELECT Retrieves Rows, Not Just Columns

A common beginner misunderstanding is thinking that `SELECT` limits rows.  
It does not.

Example:

    SELECT email
    FROM customers;

This returns:
- The `email` column
- For **every row** in the table

Row filtering is not handled by `SELECT`; that responsibility belongs to the `WHERE` clause.

---

## Selecting All Columns

You can use the asterisk (`*`) to select all columns.

Example:

    SELECT *
    FROM customers;

This returns:
- Every column
- For every row

### Gotchas with SELECT *

- The column order depends on table definition
- Queries may break if new columns are added
- More data than needed is transferred
- Not recommended for production or analytical queries

Explicitly listing columns is considered best practice.

---

## Column Order and Output Shape

The `SELECT` clause controls:
- Which columns appear
- The order of columns in the result

Example:

    SELECT email, first_name
    FROM customers;

Even if `first_name` appears before `email` in the table, the result will show `email` first.

---

## SELECT and Duplicate Data

By default, `SELECT` returns all matching rows, including duplicates.

Example:

    SELECT country
    FROM customers;

If many customers live in the same country, that country will appear multiple times.  
Removing duplicates requires an explicit instruction, not just `SELECT`.

---

## Common SELECT Gotchas

- `SELECT` does not filter rows
- `SELECT` does not change data
- Column aliases created in `SELECT` cannot be used in `WHERE`
- Column names must exist in the table
- Misspelled column names cause immediate errors

---

## Mental Model for SELECT

Think of `SELECT` as defining the **final output columns**, not the data source and not the filtering logic.

- `FROM` decides where data comes from
- `WHERE` decides which rows survive
- `SELECT` decides what each surviving row looks like

Understanding this separation is critical to writing correct SQL queries.
