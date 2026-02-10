# Using SELECT * FROM in SQL

The `SELECT * FROM` statement is a common and simple way to retrieve data from a table. It instructs the SQL server to return **all columns** from the specified table and is often one of the first queries written when exploring a database.

---

## What SELECT * FROM Means

The asterisk (`*`) is a wildcard that represents **all columns** in a table.

Basic syntax:

    SELECT * 
    FROM table_name;

This tells SQL:
- Read the specified table
- Retrieve every column
- Return the data for every matching row

---

## Retrieving an Entire Table

When `SELECT * FROM` is used **without a WHERE clause**, SQL returns the full contents of the table.

Example:

    SELECT *
    FROM rental;

This query returns:
- All columns in the `rental` table
- All rows stored in the table

The result is an exact representation of the table’s current data.

---

## Why SELECT * Is Commonly Used

`SELECT *` is useful when:
- Exploring an unfamiliar table
- Inspecting table structure and data
- Debugging or validating data during development
- Quickly viewing everything stored in a table

It provides a fast way to understand what data exists without knowing column names in advance.

---

## Important Considerations and Gotchas

While convenient, `SELECT *` has several drawbacks:

- It may return more data than needed
- Performance can suffer on large tables
- Queries can break if columns are added or removed
- Column order depends on the table definition, not the query

For production queries or analysis, explicitly listing columns is considered best practice.

---

## SELECT * and Row Filtering

`SELECT *` does not filter rows on its own.

Example:

    SELECT *
    FROM rental;

This returns every row.  
Filtering rows requires an additional clause:

    SELECT *
    FROM rental
    WHERE return_date IS NULL;

---

## Executing Queries in MySQL Workbench Tabs

MySQL Workbench allows multiple query tabs to be open at the same time.

Important behavior to understand:
- Each tab is independent
- Only the query written in the **active tab** is executed
- Queries in other tabs are ignored unless explicitly run

This makes it easy to:
- Experiment with different queries
- Compare results
- Keep reference queries open while working

---

## Mental Model for SELECT *

Think of `SELECT * FROM table_name` as asking:

“Show me everything that exists in this table right now.”

It is a powerful exploration tool, but one that should be used thoughtfully as queries become more complex and data volumes grow.
