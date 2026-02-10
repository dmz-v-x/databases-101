# Deleting Specific Records in MySQL

Deleting records is how data is **removed row by row** from a MySQL table. The `DELETE` statement allows you to precisely control **which records are removed** using a `WHERE` clause. This operation permanently removes data, so understanding how to use it safely is essential.

This guide explains how to delete specific records, common patterns, and critical safety rules.

---

## The DELETE Statement

The `DELETE` statement removes rows from a table.

Basic syntax:

    DELETE FROM table_name
    WHERE condition;

- `FROM table_name` specifies the table
- `WHERE` specifies which rows to delete

Without a `WHERE` clause, **all rows are deleted**.

---

## Deleting a Single Record

    DELETE FROM customers
    WHERE customer_id = 5;

This:
- Finds the row with `customer_id = 5`
- Deletes only that row
- Leaves all other rows untouched

This is the safest and most common delete pattern.

---

## Deleting Multiple Specific Records

    DELETE FROM customers
    WHERE customer_id IN (10, 15, 20);

This deletes multiple rows based on a list of values.

---

## Deleting Records Using Conditions

    DELETE FROM payment
    WHERE amount < 1.00;

This deletes all payments with an amount less than 1.00.

---

## Deleting Records Based on Dates

    DELETE FROM rental
    WHERE rental_date < '2005-01-01';

This removes old records based on a date condition.

---

## Deleting Records with AND / OR Conditions

    DELETE FROM payment
    WHERE amount > 10
      AND payment_date < '2006-01-01';

Only rows matching **all conditions** are deleted.

---

## Deleting Records with LIKE

    DELETE FROM customer
    WHERE email LIKE '%test%';

This deletes rows where the email contains the word `test`.

---

## Deleting Records Using a Subquery

    DELETE FROM orders
    WHERE customer_id IN (
        SELECT customer_id
        FROM customers
        WHERE is_active = 0
    );

This deletes orders belonging to inactive customers.

---

## Deleting Records Using JOIN (MySQL-Specific)

    DELETE o
    FROM orders o
    JOIN customers c
      ON o.customer_id = c.customer_id
    WHERE c.is_active = 0;

This deletes rows from `orders` based on conditions in another table.

---

## Deleting with LIMIT (MySQL Feature)

    DELETE FROM logs
    ORDER BY created_at
    LIMIT 100;

This deletes only the first 100 rows based on order.

Useful for:
- Batch deletions
- Cleaning large tables safely

---

## DELETE Without WHERE (Danger Zone)

    DELETE FROM customers;

This deletes **every row** in the table.

The table structure remains, but all data is gone.

Use this only when intentional and verified.

---

## DELETE vs TRUNCATE vs DROP

| Command | Deletes Rows | Deletes Structure |
|------|-------------|------------------|
| DELETE | Yes (conditional) | No |
| TRUNCATE | Yes (all rows) | No |
| DROP | Yes | Yes |

- `DELETE` is selective and reversible with transactions
- `TRUNCATE` is fast but deletes everything
- `DROP` removes the entire table

---

## Safe Deletion Workflow (Highly Recommended)

1. Preview rows first:

        SELECT *
        FROM customers
        WHERE is_active = 0;

2. Delete using the same condition:

        DELETE FROM customers
        WHERE is_active = 0;

3. Verify affected rows count

---

## Common DELETE Mistakes

- Forgetting the WHERE clause
- Using the wrong condition
- Deleting from the wrong table
- Not understanding foreign key constraints
- Assuming DELETE can be undone

---

## Foreign Key Constraints and DELETE

If a row is referenced by another table:
- MySQL may block the delete
- Or cascade the delete (if configured)

Always understand relationships before deleting.

---

## Best Practices for DELETE

- Always use a WHERE clause
- Run SELECT first
- Delete in small batches for large tables
- Use transactions when possible
- Never test DELETE directly in production

---

## Mental Model

Think of `DELETE` as erasing selected rows:

- The table remains
- Only matching rows are removed
- Conditions control precision
- Once deleted, data is gone

Deleting data is powerful — precision and caution are what separate safe developers from dangerous ones.
