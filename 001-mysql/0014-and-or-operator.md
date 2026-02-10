# Using AND and OR Operators in the WHERE Clause

SQL allows multiple logical conditions to be combined inside a `WHERE` clause. This is done using the `AND` and `OR` operators. These operators control how multiple conditions are evaluated together and have a significant impact on how many rows are returned.

---

## The AND Operator

The `AND` operator is used when **all conditions must be true** for a row to be included in the result set.

### Key Rule of AND

A row is returned **only if it satisfies every condition** joined by `AND`.

---

### Example 1: Filtering by Amount and Date

    SELECT
        customer_id,
        rental_id,
        amount,
        payment_date
    FROM payment
    WHERE amount = 0.99
    AND payment_date > '2006-01-01';

This query returns:
- Payments with an amount exactly equal to 0.99
- Only if the payment date is after January 1, 2006

Both conditions must be true.

---

### Example 2: Narrowing Results Further

    SELECT
        customer_id,
        rental_id,
        amount,
        payment_date
    FROM payment
    WHERE amount > 5
    AND payment_date > '2006-01-01';

This trims the dataset further by:
- Excluding smaller payments
- Excluding older records

Using `AND` always reduces or narrows the result set.

---

### Example 3: Multiple AND Conditions

    SELECT
        customer_id,
        rental_id,
        amount,
        payment_id
    FROM payment
    WHERE customer_id < 101
    AND amount >= 5
    AND payment_date > '2006-01-01';

This query returns rows only when **all three conditions** are satisfied:
- Customer ID is less than 101
- Payment amount is at least 5
- Payment date is after January 1, 2006

As more `AND` conditions are added, the dataset becomes smaller and more specific.

---

## The OR Operator

The `OR` operator is used when **any condition can be true** for a row to be included.

### Key Rule of OR

A row is returned if it satisfies **at least one** of the conditions joined by `OR`.

---

### Example: Matching Multiple Possible Values

    SELECT
        customer_id,
        rental_id,
        amount,
        payment_date
    FROM payment
    WHERE customer_id = 5
    OR customer_id = 11
    OR customer_id = 20;

This query returns:
- All payments made by customer 5
- All payments made by customer 11
- All payments made by customer 20

Only one condition needs to be true for a row to appear in the results.

---

## AND vs OR: Result Set Size

Understanding the difference between `AND` and `OR` is critical.

- `AND` **reduces** the result set  
  More conditions mean fewer matching rows.

- `OR` **expands** the result set  
  More conditions mean more matching rows.

---

## Combining Multiple Conditions Clearly

When writing queries with many conditions:
- Use `AND` to enforce strict rules
- Use `OR` to allow alternatives
- Be explicit and intentional with each condition

Each row is evaluated logically, condition by condition, to determine whether it should be included.

---

## Mental Model

- `AND` means **everything must be true**
- `OR` means **anything can be true**

Choosing the correct operator directly determines how much data your query returns and how precise your results are.
