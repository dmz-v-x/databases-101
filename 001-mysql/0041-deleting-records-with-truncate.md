# Deleting All Records Using TRUNCATE in MySQL

`TRUNCATE` is a fast and efficient way to **remove all records from a table at once**. Unlike `DELETE`, which removes rows one by one, `TRUNCATE` resets the table completely while keeping the table structure intact.

This operation is **destructive and irreversible**, so it must be used with full understanding.

---

## What TRUNCATE Does

`TRUNCATE`:
- Deletes **all rows** from a table
- Resets `AUTO_INCREMENT` counters
- Keeps the table structure (columns, indexes)
- Executes much faster than `DELETE`

Think of it as **emptying the table instantly**.

---

## Basic TRUNCATE Syntax

    TRUNCATE TABLE table_name;

Example:

    TRUNCATE TABLE customers;

This removes:
- Every row in `customers`
- All stored data

But keeps:
- Table definition
- Columns
- Indexes
- Constraints

---

## TRUNCATE vs DELETE (Critical Difference)

### DELETE without WHERE

    DELETE FROM customers;

- Deletes rows one by one
- Can be rolled back (inside a transaction)
- Does not reset AUTO_INCREMENT
- Slower on large tables

---

### TRUNCATE

    TRUNCATE TABLE customers;

- Removes all rows instantly
- Cannot be rolled back
- Resets AUTO_INCREMENT
- Much faster

---

## AUTO_INCREMENT Reset Behavior

After truncation:

    TRUNCATE TABLE orders;

If `order_id` was `AUTO_INCREMENT`, the next insert starts from `1` again.

This does **not** happen with `DELETE`.

---

## Foreign Key Restrictions

You **cannot truncate** a table if:
- It is referenced by a foreign key constraint
- Even if the referencing table is empty

MySQL will return an error.

Workarounds:
- Drop or disable foreign keys
- Truncate child tables first
- Use `DELETE` instead

---

## TRUNCATE Is Not Transactional

Important rule:

- `TRUNCATE` performs an implicit commit
- It cannot be rolled back
- It ignores transaction boundaries

Even if you run:

    START TRANSACTION;
    TRUNCATE TABLE customers;
    ROLLBACK;

The table remains empty.

---

## When You Should Use TRUNCATE

Use `TRUNCATE` when:
- Resetting development or test data
- Clearing staging tables
- Reinitializing temporary tables
- Performance matters and data loss is acceptable

---

## When You Should NOT Use TRUNCATE

Avoid `TRUNCATE` when:
- You need selective deletion
- You need rollback capability
- Foreign keys are involved
- You are working in production without backups

---

## TRUNCATE vs DROP (Quick Clarification)

| Command   | Deletes Rows | Deletes Table |
|----------|--------------|---------------|
| DELETE   | Yes          | No |
| TRUNCATE | Yes (all)    | No |
| DROP     | Yes          | Yes |

`TRUNCATE` clears data, `DROP` removes the table entirely.

---

## Common TRUNCATE Mistakes

- Running it on the wrong table
- Assuming it can be rolled back
- Forgetting foreign key dependencies
- Using it in production casually

Always double-check the table name.

---

## Safe Usage Checklist

Before running TRUNCATE:
- Confirm the environment (dev/test/prod)
- Confirm the table name
- Confirm backups exist
- Ensure no foreign key references
- Ensure data loss is acceptable

---

## Mental Model

Think of `TRUNCATE` as:
- Emptying a container instantly
- Keeping the container shape
- Throwing away everything inside permanently

`TRUNCATE` is powerful, fast, and unforgiving. Use it only when you are absolutely sure that **all data in the table can be safely discarded**.
