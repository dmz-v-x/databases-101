# Transactions in MySQL — What They Are and How to Work with Them

Transactions are one of the most important concepts in MySQL for building **safe, reliable, and consistent systems**. Any real-world application that deals with money, orders, inventory, or user state depends on transactions to prevent data corruption.

This guide explains transactions **from first principles**, how they work, how to use them in MySQL, and when they matter.

---

## What Is a Transaction?

A **transaction** is a sequence of one or more SQL operations that are treated as **a single unit of work**.

Either:
- **All operations succeed**, or
- **None of them are applied**

There is no partial success.

---

## Why Transactions Exist

Without transactions, databases can end up in an inconsistent state.

Example problem:
- Money is deducted from one account
- System crashes before adding money to another account

Transactions prevent this by ensuring **all-or-nothing execution**.

---

## The ACID Properties

Transactions follow four fundamental rules called **ACID**.

---

### Atomicity

All operations inside a transaction succeed together or fail together.

If one step fails:
- Everything is rolled back

---

### Consistency

The database moves from one valid state to another valid state.

Constraints, keys, and rules are always respected.

---

### Isolation

Transactions do not interfere with each other.

One transaction cannot see partial changes made by another.

---

### Durability

Once a transaction is committed:
- The changes are permanent
- They survive crashes or restarts

---

## Transaction Support in MySQL

Transactions are supported by **transactional storage engines**, primarily:

- InnoDB (default and recommended)
- Not supported by MyISAM

Always ensure your tables use **InnoDB**.

---

## Basic Transaction Commands

MySQL provides three core commands:

    START TRANSACTION;
    COMMIT;
    ROLLBACK;

---

## Basic Transaction Flow

    START TRANSACTION;

    UPDATE accounts
    SET balance = balance - 100
    WHERE account_id = 1;

    UPDATE accounts
    SET balance = balance + 100
    WHERE account_id = 2;

    COMMIT;

This ensures:
- Either both balance updates happen
- Or neither happens

---

## Rolling Back a Transaction

If something goes wrong:

    START TRANSACTION;

    UPDATE inventory
    SET quantity = quantity - 1
    WHERE product_id = 10;

    ROLLBACK;

After `ROLLBACK`:
- The update is undone
- Data returns to its original state

---

## What Happens Without COMMIT

If you start a transaction but do not commit it:
- Changes are not permanent
- MySQL may roll them back automatically on disconnect

---

## Autocommit Mode

By default, MySQL runs in **autocommit mode**.

This means:
- Every SQL statement is automatically committed
- Transactions are not grouped unless explicitly started

Check autocommit status:

    SELECT @@autocommit;

---

### Disabling Autocommit

    SET autocommit = 0;

Now:
- Changes persist only after `COMMIT`
- Useful for batch operations

Re-enable autocommit:

    SET autocommit = 1;

---

## Transactions with INSERT, UPDATE, DELETE

Transactions work with all data-modifying statements.

Example:

    START TRANSACTION;

    INSERT INTO orders (customer_id, total_amount)
    VALUES (5, 120.00);

    DELETE FROM cart
    WHERE customer_id = 5;

    COMMIT;

Either both happen, or neither happens.

---

## Using SAVEPOINTS

Savepoints allow partial rollbacks inside a transaction.

    START TRANSACTION;

    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

    SAVEPOINT before_credit;

    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

    ROLLBACK TO before_credit;

    COMMIT;

Result:
- First update stays
- Second update is undone

---

## Transactions and Errors

If a statement fails:
- The transaction remains active
- MySQL does not auto-rollback everything

You must explicitly:
- `ROLLBACK`, or
- Fix the issue and `COMMIT`

---

## Transactions and DDL Statements

Important rule:

- `CREATE`, `ALTER`, `DROP`, `TRUNCATE` cause **implicit commits**
- They cannot be rolled back

Example:

    START TRANSACTION;
    TRUNCATE TABLE orders;
    ROLLBACK;

The table remains empty.

---

## Isolation Levels (High-Level)

Isolation levels control how transactions interact.

Common levels:
- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ (default in MySQL)
- SERIALIZABLE

Most applications work well with the default.

---

## When You Must Use Transactions

Use transactions when:
- Money is involved
- Multiple related updates occur
- Data integrity matters
- Failure must not corrupt data
- Business rules span multiple tables

Examples:
- Payments
- Order placement
- Inventory updates
- Account transfers

---

## When Transactions Are Not Needed

Transactions may be unnecessary when:
- Single-row updates
- Read-only queries
- Non-critical logging
- Temporary data

---

## Common Transaction Mistakes

- Forgetting to use transactions
- Using non-transactional tables
- Forgetting to COMMIT
- Assuming DDL can be rolled back
- Long-running transactions causing locks

---

## Best Practices for Transactions

- Keep transactions short
- Always handle errors
- Commit as soon as possible
- Avoid user interaction inside transactions
- Use InnoDB
- Never mix DDL inside transactions

---

## Mental Model

Think of a transaction as a **protected box**:

- Changes go into the box
- COMMIT opens the box and applies changes
- ROLLBACK throws the box away

Transactions are what make MySQL safe for real-world systems. Without them, even simple applications eventually corrupt their data.
