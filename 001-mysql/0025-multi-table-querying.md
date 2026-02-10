# Multi-Table Querying in SQL

Relational databases are designed so that data is spread across multiple tables rather than stored in a single large table. This structure reduces duplication and improves data integrity. To work with related data stored in different tables, SQL provides **multi-table querying**, most commonly through **JOINs**.

Multi-table querying allows you to combine data from multiple tables into a single result set based on defined relationships.

---

## Why Data Is Split Across Tables

In a normalized database:
- Each table represents a single entity (customers, orders, payments, films, etc.)
- Related data is connected using keys
- Tables reference each other using foreign keys

Because of this design, meaningful questions often require data from more than one table.

Example questions:
- Which customers made which payments?
- Which films belong to which categories?
- Which rentals were handled by which staff members?

Answering these requires querying multiple tables together.

---

## How Tables Are Related

Tables are connected using:
- **Primary keys**: uniquely identify rows in a table
- **Foreign keys**: reference primary keys in another table

Example relationship:
- `customer.customer_id` (primary key)
- `payment.customer_id` (foreign key)

This relationship allows payments to be linked back to customers.

---

## Basic Structure of a Multi-Table Query

Multi-table queries use the `FROM` clause combined with `JOIN`.

General syntax:

    SELECT column_list
    FROM table_one
    JOIN table_two
      ON join_condition;

The `ON` clause defines **how the tables are related**.

---

## INNER JOIN

An `INNER JOIN` returns only rows where a matching record exists in **both tables**.

Example:

    SELECT
        c.customer_id,
        c.first_name,
        c.last_name,
        p.amount
    FROM customer c
    INNER JOIN payment p
      ON c.customer_id = p.customer_id;

This query:
- Matches customers to their payments
- Excludes customers with no payments
- Excludes payments that do not match a customer

`INNER JOIN` is the most commonly used join type.

---

## LEFT JOIN

A `LEFT JOIN` returns:
- All rows from the left table
- Matching rows from the right table
- NULL values where no match exists

Example:

    SELECT
        c.customer_id,
        c.first_name,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id;

This query:
- Returns all customers
- Includes payments where they exist
- Shows NULL for customers with no payments

`LEFT JOIN` is useful for finding missing or optional relationships.

---

## RIGHT JOIN

A `RIGHT JOIN` is the opposite of a `LEFT JOIN`.

Example:

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    RIGHT JOIN payment p
      ON c.customer_id = p.customer_id;

This returns:
- All payments
- Matching customers where they exist

In practice, `RIGHT JOIN` is rarely used because the same result can usually be achieved with a `LEFT JOIN` by switching table order.

---

## Joining More Than Two Tables

SQL allows joining multiple tables in a single query.

Example:

    SELECT
        c.customer_id,
        f.title,
        p.amount
    FROM customer c
    JOIN rental r
      ON c.customer_id = r.customer_id
    JOIN inventory i
      ON r.inventory_id = i.inventory_id
    JOIN film f
      ON i.film_id = f.film_id
    JOIN payment p
      ON r.rental_id = p.rental_id;

This query:
- Connects customers to rentals
- Rentals to inventory
- Inventory to films
- Rentals to payments

Each join builds on the previous relationship.

---

## Using Table Aliases

Table aliases make multi-table queries easier to read and write.

Example:

    FROM customer c
    JOIN payment p
      ON c.customer_id = p.customer_id;

Aliases:
- Reduce repetition
- Improve clarity
- Are essential in complex queries

---

## Filtering in Multi-Table Queries

Filtering is done using `WHERE`, just like single-table queries.

Example:

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE p.amount > 5;

Filtering happens **after joins are applied**, unless join conditions themselves restrict rows.

---

## Common Gotchas in Multi-Table Querying

- Forgetting the join condition leads to a Cartesian product
- Joining on the wrong columns produces incorrect results
- Using `WHERE` instead of `ON` can accidentally turn outer joins into inner joins
- Ambiguous column names require table prefixes
- Joining many large tables can impact performance

---

## Mental Model

Think of multi-table querying as building a path through related data:

- Each table is a stop
- Foreign keys are the roads
- JOINs define which roads to take
- The final result combines information from all stops

Mastering multi-table querying is essential for working with real-world relational databases, where meaningful insights almost always span more than one table.
