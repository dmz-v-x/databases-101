# Updating Data Records in MySQL

Updating records is how existing data is modified in a MySQL table. The `UPDATE` statement allows you to change one or more column values for selected rows. This is a **powerful and potentially dangerous operation**, so understanding how it works is critical.

This guide explains how `UPDATE` works, how to use it safely, and common patterns you will use in real applications.

---

## The UPDATE Statement

The `UPDATE` statement modifies existing rows in a table.

Basic syntax:

    UPDATE table_name
    SET column_name = new_value
    WHERE condition;

The `SET` clause defines **what changes**, and the `WHERE` clause defines **which rows change**.

---

## Updating a Single Column

    UPDATE customers
    SET email = 'new.email@example.com'
    WHERE customer_id = 5;

This:
- Finds the customer with `customer_id = 5`
- Updates only their email address
- Leaves all other rows unchanged

---

## Updating Multiple Columns

    UPDATE customers
    SET first_name = 'John',
        last_name = 'Smith'
    WHERE customer_id = 10;

Multiple columns can be updated in a single statement.

---

## Updating Multiple Rows

    UPDATE customers
    SET is_active = 0
    WHERE last_login < '2022-01-01';

This updates **all rows** that match the condition.

---

## Updating Without a WHERE Clause (Danger Zone)

    UPDATE customers
    SET is_active = 0;

This updates **every row in the table**.

This is valid SQL, but extremely dangerous in production.  
Always double-check for a `WHERE` clause before executing.

---

## Using UPDATE with Conditions

You can use all standard `WHERE` operators:

    UPDATE payment
    SET amount = amount * 1.1
    WHERE amount < 5;

This increases small payments by 10%.

---

## Using UPDATE with ORDER BY and LIMIT (MySQL Feature)

MySQL allows limiting updates.

    UPDATE customers
    SET is_active = 0
    ORDER BY created_at
    LIMIT 10;

This updates only the first 10 rows according to the order.

---

## Updating Using Values from Another Table

### UPDATE with JOIN

    UPDATE orders o
    JOIN customers c
      ON o.customer_id = c.customer_id
    SET o.status = 'priority'
    WHERE c.vip = 1;

This updates orders based on customer attributes.

---

## Updating with Subqueries

    UPDATE products
    SET price = price * 1.05
    WHERE product_id IN (
        SELECT product_id
        FROM promotions
    );

This updates products that appear in another table.

---

## Handling NULL Values in UPDATE

Setting a column to NULL:

    UPDATE customers
    SET phone_number = NULL
    WHERE customer_id = 15;

Using conditional updates:

    UPDATE customers
    SET phone_number = 'N/A'
    WHERE phone_number IS NULL;

---

## UPDATE with CASE

    UPDATE payment
    SET amount =
        CASE
            WHEN amount < 2 THEN amount * 1.2
            WHEN amount BETWEEN 2 AND 5 THEN amount * 1.1
            ELSE amount
        END;

This applies different logic depending on existing values.

---

## Common UPDATE Errors

- Forgetting the WHERE clause
- Updating the wrong rows due to incorrect conditions
- Violating NOT NULL or UNIQUE constraints
- Data type mismatches
- Updating primary keys unintentionally

---

## Best Practices for Updating Data

- Always run a SELECT with the same WHERE clause first
- Use transactions for critical updates
- Update small batches when possible
- Avoid updating primary keys
- Backup data before large updates

Example safe workflow:

    SELECT * FROM customers WHERE is_active = 0;

    UPDATE customers
    SET status = 'inactive'
    WHERE is_active = 0;

---

## UPDATE vs INSERT vs DELETE

| Operation | Purpose |
|---------|--------|
| INSERT | Add new rows |
| UPDATE | Modify existing rows |
| DELETE | Remove rows |

Each operation changes data in a different way.

---

## Mental Model

Think of `UPDATE` as editing rows in place:

- The table stays the same
- Rows are modified, not replaced
- `WHERE` decides which rows are touched
- `SET` decides how they change

Mastering `UPDATE` is essential for maintaining data correctness and safely evolving application state in MySQL.
