# The FROM Clause in SQL

The `FROM` clause defines **where the data comes from**. It tells the SQL server which table should be read in order to retrieve the data requested by the query. Along with `SELECT`, it forms the minimum structure required for any meaningful SQL statement.

---

## What FROM Does

The `FROM` clause identifies the table that contains the data you want to work with.

Basic syntax:

    FROM table_name

Used together with `SELECT`:

    SELECT column_name
    FROM table_name;

This instructs the SQL server to:
- Locate the specified table
- Read its rows
- Make its columns available to the query

---

## FROM Is Required in Every Query

Every SQL query must include:
- `SELECT` to define what data to return
- `FROM` to define where that data lives

Minimal valid query:

    SELECT column_name
    FROM table_name;

Without `FROM`, SQL has no source for the data.  
Without `SELECT`, SQL has no output to return.

---

## FROM Works at the Table Level

Unlike `SELECT`, which can list multiple columns, `FROM` works at the **table level**.

Example:

    SELECT first_name, last_name
    FROM customers;

The SQL server:
- Loads the `customers` table
- Makes all its rows and columns available
- Allows `SELECT` and other clauses to operate on that data

---

## Using Multiple Tables in FROM

You cannot freely list multiple tables separated by commas in modern SQL.

Incorrect approach:

    SELECT *
    FROM customers, orders;

While some databases may allow this syntax, it is ambiguous, harder to read, and not recommended.

To work with multiple tables, SQL requires the use of **JOINs**, which explicitly define how tables are related.

Example (conceptual):

    SELECT *
    FROM customers
    JOIN orders
      ON customers.id = orders.customer_id;

Joins make relationships clear and prevent accidental data multiplication.

---

## Why Joins Are Required

Requiring joins ensures:
- Clear relationships between tables
- Predictable query results
- Better readability and maintainability
- Reduced risk of incorrect data combinations

Joins will be covered in detail separately, but it is important to understand that `FROM` only defines the starting table, not how tables connect.

---

## FROM and Query Execution

The `FROM` clause is the first step in how SQL processes a query.

Conceptually:
- SQL starts by loading the table(s) defined in `FROM`
- Only then do filtering, grouping, and selection occur

This is why `FROM` appears early in the query structure, even though it is written after `SELECT`.

---

## Common FROM Gotchas

- Table names must exist in the database schema
- Table names must be spelled correctly
- You cannot access columns that do not belong to the table(s) in `FROM`
- Multiple tables require joins, not comma-separated lists

---

## Mental Model for FROM

Think of `FROM` as the **data source selector**.

- It defines which table is brought into memory
- All other clauses operate on the data provided by `FROM`
- Without it, the query has nothing to work with

Understanding `FROM` is essential, because every SQL query begins by defining where its data comes from.
