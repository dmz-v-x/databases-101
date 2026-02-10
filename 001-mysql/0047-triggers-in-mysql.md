# Triggers in MySQL — What They Are and How to Work with Them

Triggers are a MySQL feature that allow you to **automatically execute SQL logic when a specific database event occurs**. They are powerful, implicit, and must be used carefully because they run **without being explicitly called**.

This guide explains triggers from first principles: what they are, why they exist, how to create and use them, and when to avoid them.

---

## What Is a Trigger?

A **trigger** is a block of SQL code that automatically runs **in response to an event** on a table.

Triggers execute when one of these events happens:
- `INSERT`
- `UPDATE`
- `DELETE`

They are tied to a **specific table** and fire automatically when the event occurs.

You never call a trigger manually.

---

## Why Triggers Exist

Triggers exist to enforce **automatic, consistent behavior** at the database level.

They are commonly used to:
- Maintain audit logs
- Enforce complex business rules
- Automatically update related data
- Validate or transform data
- Keep derived data in sync

Triggers ensure that certain rules are **never bypassed**, even if data is modified outside the application.

---

## Key Characteristics of Triggers

- Automatically executed
- Tied to a table and event
- Cannot be called directly
- Run inside the transaction that triggered them
- Can modify data
- Can affect performance if overused

---

## Trigger Timing: BEFORE vs AFTER

Every trigger runs either **before** or **after** the triggering event.

---

### BEFORE Triggers

Executed **before** the data change happens.

Use cases:
- Validate data
- Modify values before insert/update
- Prevent invalid operations

Example idea:
- Set default values
- Reject invalid input

---

### AFTER Triggers

Executed **after** the data change happens.

Use cases:
- Logging
- Auditing
- Updating related tables

Example idea:
- Insert audit record after data change

---

## Trigger Events

Triggers respond to three events:

- `INSERT`
- `UPDATE`
- `DELETE`

Each event can have:
- A `BEFORE` trigger
- An `AFTER` trigger

Maximum:
- 6 triggers per table (BEFORE/AFTER × INSERT/UPDATE/DELETE)

---

## Trigger Syntax

    CREATE TRIGGER trigger_name
    timing event
    ON table_name
    FOR EACH ROW
    BEGIN
        SQL statements;
    END;

Components:
- `trigger_name` → unique name
- `timing` → BEFORE or AFTER
- `event` → INSERT, UPDATE, or DELETE
- `FOR EACH ROW` → trigger runs once per affected row

---

## Important: DELIMITER

Triggers contain multiple SQL statements, so the delimiter must be changed.

    DELIMITER //

    CREATE TRIGGER trigger_name
    ...
    END //

    DELIMITER ;

---

## Using OLD and NEW Keywords

Triggers can access row values using:

- `NEW.column_name` → new row values
- `OLD.column_name` → existing row values

Availability:
- INSERT → `NEW` only
- DELETE → `OLD` only
- UPDATE → both `OLD` and `NEW`

---

## Example: BEFORE INSERT Trigger

Automatically set a created timestamp.

    DELIMITER //

    CREATE TRIGGER before_customer_insert
    BEFORE INSERT ON customers
    FOR EACH ROW
    BEGIN
        SET NEW.created_at = NOW();
    END //

    DELIMITER ;

This runs before each insert.

---

## Example: AFTER INSERT Trigger (Audit Log)

    DELIMITER //

    CREATE TRIGGER after_order_insert
    AFTER INSERT ON orders
    FOR EACH ROW
    BEGIN
        INSERT INTO order_audit (order_id, action, action_time)
        VALUES (NEW.order_id, 'INSERT', NOW());
    END //

    DELIMITER ;

This logs every new order automatically.

---

## Example: BEFORE UPDATE Trigger (Validation)

Prevent negative balances.

    DELIMITER //

    CREATE TRIGGER before_account_update
    BEFORE UPDATE ON accounts
    FOR EACH ROW
    BEGIN
        IF NEW.balance < 0 THEN
            SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Balance cannot be negative';
        END IF;
    END //

    DELIMITER ;

This blocks invalid updates.

---

## Example: AFTER DELETE Trigger

Log deletions.

    DELIMITER //

    CREATE TRIGGER after_customer_delete
    AFTER DELETE ON customers
    FOR EACH ROW
    BEGIN
        INSERT INTO customer_log (customer_id, deleted_at)
        VALUES (OLD.customer_id, NOW());
    END //

    DELIMITER ;

---

## Viewing Triggers

List triggers:

    SHOW TRIGGERS;

View trigger definition:

    SHOW CREATE TRIGGER trigger_name;

---

## Dropping a Trigger

    DROP TRIGGER IF EXISTS trigger_name;

Triggers must be dropped explicitly.

---

## Triggers and Transactions

Triggers:
- Run inside the same transaction as the triggering query
- Are rolled back if the transaction rolls back
- Cannot commit or rollback independently

This ensures consistency.

---

## What Triggers Cannot Do

Triggers cannot:
- Commit or rollback transactions
- Call stored procedures that return result sets
- Modify the same table recursively
- Replace application logic entirely

---

## Common Trigger Mistakes

- Hiding business logic in triggers
- Creating complex logic that is hard to debug
- Forgetting triggers exist
- Causing performance issues
- Creating unexpected side effects
- Overusing triggers instead of clear application logic

---

## When You SHOULD Use Triggers

Use triggers when:
- Logic must always run
- Data integrity must be enforced centrally
- Auditing is mandatory
- Automatic side effects are required

---

## When You Should NOT Use Triggers

Avoid triggers when:
- Logic is complex and evolving
- Debugging clarity is important
- Business rules belong in application code
- Performance is critical and predictable behavior is needed

---

## Triggers vs Stored Procedures

| Aspect | Trigger | Stored Procedure |
|-----|--------|----------------|
| Execution | Automatic | Explicit (CALL) |
| Control | Implicit | Explicit |
| Debugging | Harder | Easier |
| Use case | Enforcement, auditing | Business workflows |

---

## Best Practices for Triggers

- Keep triggers small and focused
- Use clear, descriptive names
- Document trigger behavior
- Avoid heavy logic
- Avoid chaining triggers
- Audit and review regularly

---

## Mental Model

Think of triggers as **automatic guards**:

- You don’t call them
- They react to changes
- They enforce rules silently
- They protect the database

Triggers are powerful and dangerous at the same time. Used sparingly and intentionally, they add safety and consistency. Used carelessly, they make systems unpredictable and hard to debug.
