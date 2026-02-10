# The USE Statement in SQL

When working with MySQL Workbench, databases are organized into schemas. Before running queries on tables, the SQL server must know **which schema you want to work with**. The `USE` statement defines this context.

---

## What the USE Statement Does

The `USE` statement tells MySQL which database (schema) should be considered the active one for the current session.

Basic syntax:

    USE schema_name;

Example:

    USE mavenmovies;

After executing this command, all subsequent queries run against the `mavenmovies` schema unless another schema is selected.

---

## Why USE Is Important

A MySQL server can host many databases at the same time. Without explicitly selecting one, MySQL has no way of knowing which database contains the tables referenced in a query.

If a query is executed without an active schema, MySQL returns an error similar to:

    No database selected

This error simply means that MySQL does not yet know which database you intend to use.

---

## How USE Affects Queries

Once a schema is selected using `USE`, table names can be referenced directly.

Example:

    USE mavenmovies;

    SELECT *
    FROM rental;

Without the `USE` statement, the same query would fail unless the schema name is explicitly included.

---

## Alternative to USE

Instead of using `USE`, tables can be referenced with fully qualified names.

Example:

    SELECT *
    FROM mavenmovies.rental;

While this works, using `USE` is often cleaner and more convenient when running multiple queries against the same schema.

---

## SQL Statement Termination with Semicolons

In SQL, the semicolon (`;`) marks the **end of a statement**.

Example:

    USE mavenmovies;
    SELECT * FROM rental;

Each statement is clearly separated and executed independently.

---

## Why Semicolons Matter

- They allow multiple SQL statements to be written in a single script
- They help the SQL engine know when a command is complete
- Some tools require semicolons to execute statements correctly

In MySQL Workbench, a semicolon ensures that the correct statement is executed, especially when multiple commands are present in the same editor tab.

---

## Common Gotchas

- Forgetting to run `USE` leads to “no database selected” errors
- Semicolons are required when executing multiple statements together
- Changing schemas requires another `USE` statement
- `USE` applies only to the current connection session

---

## Mental Model for USE

Think of `USE` as setting your current working folder.

- The database is the folder
- Tables are the files inside it
- `USE` tells SQL which folder to look in by default

Once the schema is set, SQL queries can focus purely on data retrieval and manipulation.
