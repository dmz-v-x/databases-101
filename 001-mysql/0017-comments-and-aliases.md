# Comments and Aliases in SQL

Comments and aliases are not used to retrieve data, but they are essential for writing **clear, readable, and maintainable SQL**. They help document intent, improve query readability, and make results easier to understand.

---

## Comments in SQL

Comments are used to explain what a query or part of a query does. They are ignored by the SQL engine and exist only for humans reading the code.

SQL supports two main types of comments.

---

## Single-Line Comments

Single-line comments begin with two hyphens (`--`). Everything after `--` on the same line is treated as a comment.

Example:

    -- This query retrieves all films
    SELECT *
    FROM film;

Single-line comments are commonly used to:
- Describe the purpose of a query
- Temporarily disable a line of SQL
- Add short explanations inline

Example:

    SELECT title, rating  -- selecting title and rating columns
    FROM film;

---

## Multi-Line Comments

Multi-line comments begin with `/*` and end with `*/`. Everything inside is ignored by SQL.

Example:

    /*
      This query groups films by rating
      and counts how many films exist
      in each category
    */
    SELECT rating, COUNT(film_id)
    FROM film
    GROUP BY rating;

Multi-line comments are useful for:
- Explaining complex logic
- Documenting assumptions
- Commenting out large sections of SQL

---

## Aliases in SQL

Aliases are temporary names given to columns or tables in a query. They exist only for the duration of the query and do not change the database structure.

Aliases are created using the `AS` keyword, although `AS` is optional.

---

## Column Aliases

Column aliases rename columns in the result set to make them more readable.

Example without alias:

    SELECT COUNT(film_id)
    FROM film;

The column name in the result may appear unclear or system-generated.

Example with alias:

    SELECT COUNT(film_id) AS total_films
    FROM film;

The result column is clearly labeled as `total_films`.

---

## Aliases Without AS

The `AS` keyword is optional in most SQL dialects.

Example:

    SELECT COUNT(film_id) total_films
    FROM film;

This behaves the same as using `AS`.

---

## Aliases with Expressions

Aliases are especially useful when working with calculations or aggregate functions.

Example:

    SELECT customer_id, SUM(amount) AS total_spent
    FROM payment
    GROUP BY customer_id;

The alias makes the meaning of the calculated column immediately clear.

---

## Table Aliases

Table aliases provide short names for tables, which is especially helpful in queries involving joins or long table names.

Example:

    SELECT p.payment_id, p.amount
    FROM payment AS p;

Using table aliases:
- Reduces typing
- Improves readability
- Makes complex queries easier to follow

---

## Aliases and Query Order

Aliases defined in the `SELECT` clause:
- Can be used in `ORDER BY`
- Cannot be used in `WHERE`
- Are not available before `SELECT` is evaluated

Example:

    SELECT amount * 1.18 AS total_with_tax
    FROM payment
    ORDER BY total_with_tax;

This works because `ORDER BY` is evaluated after `SELECT`.

---

## Common Gotchas with Comments and Aliases

- Comments do not affect query execution
- Aliases do not exist outside the query
- Aliases cannot be referenced in `WHERE`
- Aliases improve result readability but do not rename columns in the table

---

## Mental Model

- Comments explain *why* the SQL exists
- Aliases explain *what* the results represent

Using comments and aliases consistently makes SQL easier to read, easier to debug, and easier to maintain, especially as queries grow in size and complexity.
