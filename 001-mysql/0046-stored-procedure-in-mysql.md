# Stored Procedures in MySQL — What They Are and How to Work with Them

Stored procedures are a powerful MySQL feature that allow you to **store reusable SQL logic directly inside the database**. They are commonly used to encapsulate business logic, reduce repetition, improve performance, and enforce consistent behavior across applications.

This guide explains stored procedures **from fundamentals to practical usage**, step by step.

---

## What Is a Stored Procedure?

A **stored procedure** is a named block of SQL statements that:
- Is stored in the database
- Can accept input parameters
- Can return output values
- Can be executed repeatedly

Think of a stored procedure as:
- A function for your database
- A reusable SQL program
- A predefined sequence of SQL commands

---

## Why Stored Procedures Exist

Without stored procedures:
- SQL logic is repeated in application code
- Complex operations are rewritten many times
- Business rules are scattered across services

Stored procedures help by:
- Centralizing logic in the database
- Reducing duplication
- Improving maintainability
- Reducing network round-trips
- Enforcing consistent rules

---

## When Stored Procedures Are Used

Stored procedures are commonly used for:
- Multi-step database operations
- Business logic enforcement
- Data validation
- Batch processing
- Reporting logic
- Legacy systems and enterprise environments

They are less common in modern microservice-heavy architectures but still very relevant.

---

## Basic Structure of a Stored Procedure

    CREATE PROCEDURE procedure_name ()
    BEGIN
        SQL statements;
    END;

This defines:
- A procedure name
- A block of SQL statements
- Logic stored inside the database

---

## Important: DELIMITER Concept

MySQL uses `;` to end statements.  
Stored procedures contain multiple statements, so we must temporarily change the delimiter.

Example:

    DELIMITER //

    CREATE PROCEDURE get_all_customers()
    BEGIN
        SELECT * FROM customers;
    END //

    DELIMITER ;

This allows MySQL to understand where the procedure ends.

---

## Creating a Simple Stored Procedure

    DELIMITER //

    CREATE PROCEDURE get_all_customers()
    BEGIN
        SELECT * FROM customers;
    END //

    DELIMITER ;

This procedure:
- Has no parameters
- Always returns all customers

---

## Executing a Stored Procedure

    CALL get_all_customers();

This runs the procedure just like a query.

---

## Stored Procedures with Input Parameters

Procedures can accept input values.

    DELIMITER //

    CREATE PROCEDURE get_customer_by_id(IN cid INT)
    BEGIN
        SELECT *
        FROM customers
        WHERE customer_id = cid;
    END //

    DELIMITER ;

Execution:

    CALL get_customer_by_id(5);

---

## Types of Parameters

MySQL supports three parameter types:

- `IN`  → Input only
- `OUT` → Output only
- `INOUT` → Input and output

---

## Stored Procedures with Output Parameters

    DELIMITER //

    CREATE PROCEDURE get_total_customers(OUT total INT)
    BEGIN
        SELECT COUNT(*) INTO total
        FROM customers;
    END //

    DELIMITER ;

Execution:

    CALL get_total_customers(@count);
    SELECT @count;

---

## Using Variables Inside Stored Procedures

    DECLARE total_payments DECIMAL(10,2);

Variables must be declared:
- At the top of the `BEGIN` block
- Before executable statements

Example:

    DELIMITER //

    CREATE PROCEDURE get_total_revenue()
    BEGIN
        DECLARE total DECIMAL(10,2);

        SELECT SUM(amount)
        INTO total
        FROM payment;

        SELECT total;
    END //

    DELIMITER ;

---

## Using IF Statements in Stored Procedures

    IF condition THEN
        statements;
    ELSE
        statements;
    END IF;

Example:

    IF amount > 100 THEN
        SET status = 'high';
    ELSE
        SET status = 'normal';
    END IF;

---

## Using CASE in Stored Procedures

    CASE
        WHEN condition THEN statements;
        WHEN condition THEN statements;
        ELSE statements;
    END CASE;

---

## Using Loops in Stored Procedures

### WHILE Loop

    WHILE condition DO
        statements;
    END WHILE;

### LOOP

    LOOP
        statements;
        IF condition THEN
            LEAVE loop_label;
        END IF;
    END LOOP;

---

## Stored Procedures and Transactions

Stored procedures can use transactions.

    START TRANSACTION;

    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;

    COMMIT;

This ensures atomic behavior.

---

## Modifying a Stored Procedure

MySQL does **not** support `ALTER PROCEDURE` for logic changes.

You must:
- Drop the procedure
- Recreate it

    DROP PROCEDURE IF EXISTS get_all_customers;

---

## Viewing Stored Procedures

    SHOW PROCEDURE STATUS;

    SHOW CREATE PROCEDURE get_all_customers;

---

## Permissions and Security

Stored procedures can:
- Restrict direct table access
- Expose controlled operations
- Run with definer privileges

This is useful in enterprise systems.

---

## Advantages of Stored Procedures

- Reusable logic
- Better performance for complex operations
- Reduced application complexity
- Centralized business rules
- Improved security control

---

## Disadvantages of Stored Procedures

- Harder to debug
- Database vendor lock-in
- Version control challenges
- Logic split between app and DB

Modern systems often balance these trade-offs.

---

## Stored Procedures vs Application Code

| Aspect | Stored Procedure | App Code |
|------|-----------------|----------|
| Location | Database | Application |
| Performance | High | Depends on network |
| Portability | Low | High |
| Debugging | Harder | Easier |
| Business logic | Centralized | Distributed |

---

## Best Practices for Stored Procedures

- Keep procedures focused
- Avoid overly complex logic
- Use meaningful names
- Document procedures
- Avoid UI-related logic
- Use transactions carefully
- Version control procedure definitions

---

## Mental Model

Think of a stored procedure as:
- A **named SQL workflow**
- Executed with parameters
- Stored close to the data
- Enforcing rules consistently

Stored procedures are not mandatory for every project, but when used correctly, they are a powerful tool for building reliable, efficient MySQL-based systems.
