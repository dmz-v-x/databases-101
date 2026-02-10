# Multi-Condition JOINs in SQL

A **multi-condition JOIN** is a JOIN that uses **more than one condition** in its `ON` clause to determine how rows from two tables should be matched. This is common in real-world databases where relationships are defined by **multiple columns**, not just a single key.

Multi-condition joins provide **more precise control** over how data is combined and help prevent incorrect or duplicate matches.

---

## Why Multi-Condition JOINs Are Needed

In many schemas:
- A single column is not enough to uniquely identify a relationship
- Tables may share more than one related attribute
- Additional constraints are required to ensure correctness

Examples:
- Matching records by both `id` and `date`
- Matching by `customer_id` and `store_id`
- Matching by `order_id` and `product_id`

Using multiple conditions ensures that rows are matched **only when all relationship rules are satisfied**.

---

## Basic Syntax of a Multi-Condition JOIN

    SELECT column_list
    FROM table_one
    JOIN table_two
      ON condition_one
     AND condition_two
     AND condition_three;

Each condition must evaluate to true for the rows to be joined.

---

## Example: Joining on Two Columns

    SELECT
        r.rental_id,
        r.rental_date,
        p.amount
    FROM rental r
    INNER JOIN payment p
      ON r.rental_id = p.rental_id
     AND r.customer_id = p.customer_id;

This query:
- Matches rentals to payments
- Ensures the rental and payment belong to the same customer
- Prevents incorrect matches when IDs overlap

Both conditions must be true for a row to appear.

---

## Example: LEFT JOIN with Multiple Conditions

    SELECT
        c.customer_id,
        p.amount
    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id
     AND p.amount > 5;

This query:
- Returns all customers
- Includes only payments greater than 5
- Preserves customers with no qualifying payments

Placing conditions in the `ON` clause preserves LEFT JOIN behavior.

---

## Multi-Condition JOIN Across Business Rules

    SELECT
        o.order_id,
        o.order_date,
        s.shipment_date
    FROM orders o
    JOIN shipments s
      ON o.order_id = s.order_id
     AND o.store_id = s.store_id
     AND s.status = 'DELIVERED';

This ensures:
- Orders are matched to shipments
- Both belong to the same store
- Only delivered shipments are included

---

## JOIN Conditions vs WHERE Conditions

JOIN conditions define **how tables relate**.  
WHERE conditions define **which rows are kept after joining**.

Incorrect pattern:

    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id
    WHERE p.amount > 5;

This converts the LEFT JOIN into an INNER JOIN.

Correct pattern:

    FROM customer c
    LEFT JOIN payment p
      ON c.customer_id = p.customer_id
     AND p.amount > 5;

---

## Using OR in JOIN Conditions

Multiple conditions can also be combined using `OR`, though this should be done carefully.

    SELECT *
    FROM employee e
    JOIN manager m
      ON e.manager_id = m.employee_id
      OR e.department_id = m.department_id;

This can:
- Increase result size dramatically
- Introduce unexpected matches

`OR` in JOINs should be used only when logically required.

---

## Common Multi-Condition JOIN Gotchas

- Forgetting one condition causes incorrect matches
- Placing conditions in `WHERE` breaks outer joins
- Using `OR` can explode result sets
- Missing table aliases causes ambiguity
- Conditions must reflect true business relationships

---

## Mental Model

Think of a multi-condition JOIN as a **multi-lock system**:

- Each condition is a lock
- All locks must open for a match
- More conditions mean stricter matching

Multi-condition JOINs are essential for accuracy when working with complex, real-world relational data.
