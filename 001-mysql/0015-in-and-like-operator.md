# IN and LIKE Operators in SQL

SQL provides specialized operators that make filtering data more expressive and readable. Two of the most commonly used are the `IN` operator, which simplifies multiple equality checks, and the `LIKE` operator, which enables pattern-based matching.

---

## The IN Operator

The `IN` operator is used when a column needs to be compared against **multiple possible values**. It is especially useful when several `OR` conditions reference the same column.

### Why Use IN

Writing multiple `OR` conditions can be repetitive and hard to read. The `IN` operator provides a cleaner and more maintainable alternative.

---

### Using OR with Multiple Conditions

    SELECT
        customer_id,
        rental_id,
        amount,
        payment_date
    FROM payment
    WHERE amount > 5
       OR customer_id = 42
       OR customer_id = 53
       OR customer_id = 60
       OR customer_id = 75;

This query returns rows where:
- The payment amount is greater than 5, or
- The customer ID matches any of the listed values

---

### Using IN for the Same Logic

    SELECT
        customer_id,
        rental_id,
        amount,
        payment_date
    FROM payment
    WHERE amount > 5
       OR customer_id IN (42, 53, 60, 75);

This version produces **identical results** but is:
- Easier to read
- Easier to write
- Easier to maintain

---

### IN Operator Gotchas

- All values inside `IN()` must be compatible with the column’s data type
- `IN()` checks for exact matches only
- An empty `IN()` list results in no matches

---

## The LIKE Operator

The `LIKE` operator allows pattern matching instead of exact value comparison. It is primarily used with text-based columns.

### Basic LIKE Syntax

    WHERE column_name LIKE 'pattern';

Patterns are built using wildcard characters.

---

## Wildcards Used with LIKE

### Percent Sign (%)

`%` matches **any number of characters**, including zero characters.

Example:

    WHERE name LIKE 'Denise%';

This matches:
- Denise
- Denise Smith
- Denise123

---

### Underscore (_)

`_` matches **exactly one character**.

Example:

    WHERE first_name LIKE '_erry';

This matches:
- Jerry
- Berry

It does not match:
- Perry Jr. (extra characters)
- Terry123 (too many characters)

---

## Case Sensitivity in LIKE

When using `LIKE`, capitalization matters depending on collation settings.

Example:

    WHERE name LIKE 'Denise%';

is different from:

    WHERE name LIKE 'denise%';

If the database is case-sensitive, these two patterns will return different results.

---

## NOT LIKE Operator

The `NOT LIKE` operator is used to **exclude rows** that match a pattern.

Example:

    SELECT *
    FROM customer
    WHERE last_name NOT LIKE 'S%';

This returns all customers whose last name does not start with the letter `S`.

`NOT LIKE` follows the same rules as `LIKE`:
- Same wildcards
- Same case-sensitivity behavior

---

## When to Use IN vs LIKE

- Use `IN` when matching against a known set of exact values
- Use `LIKE` when matching text patterns
- Use `NOT LIKE` to filter out unwanted patterns

---

## Mental Model

- `IN` asks: “Is this value one of these?”
- `LIKE` asks: “Does this value look like this pattern?”
- `NOT LIKE` asks: “Does this value not look like this pattern?”

These operators make SQL queries clearer, shorter, and more expressive when filtering complex datasets.
