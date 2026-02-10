# Logical Operators in the WHERE Clause

The `WHERE` clause filters rows in a result set by applying logical conditions. These conditions are built using comparison and matching operators. Each operator evaluates a column’s value and determines whether a row should be included in the final output.

---

## Equality Operator (=)

The equals operator returns rows where a column value matches a specified value exactly.

    SELECT *
    FROM film
    WHERE rating = 'PG';

This query returns only films with a rating of `PG`.

---

## Not Equal Operator (<>)

The not-equal operator excludes rows that match a specific value.

    SELECT *
    FROM customer
    WHERE active <> 1;

This returns customers who are not marked as active.

---

## Greater Than Operator (>)

The greater-than operator filters rows where a value exceeds the given threshold.

    SELECT *
    FROM payment
    WHERE amount > 5.99;

Only payments greater than 5.99 are returned.

---

## Less Than Operator (<)

The less-than operator filters rows where a value is below a given number.

    SELECT *
    FROM payment
    WHERE amount < 2.00;

This returns payments less than 2.00.

---

## Greater Than or Equal To (>=)

This operator includes rows where the value is either greater than or equal to the specified value.

    SELECT *
    FROM rental
    WHERE rental_duration >= 7;

This returns rentals that lasted seven days or longer.

---

## Less Than or Equal To (<=)

This operator includes rows where the value is either less than or equal to the specified value.

    SELECT *
    FROM inventory
    WHERE store_id <= 2;

This returns inventory records belonging to store 1 or store 2.

---

## BETWEEN Operator

The `BETWEEN` operator filters values within a specific range, including both boundary values.

    SELECT *
    FROM rental
    WHERE rental_date BETWEEN '2006-01-01' AND '2006-06-01';

This returns rentals that occurred within the specified date range.

---

## LIKE Operator

The `LIKE` operator filters rows by matching text patterns.

Common wildcards:
- `%` matches any number of characters
- `_` matches exactly one character

Example using a starting pattern:

    SELECT *
    FROM customer
    WHERE last_name LIKE 'Sm%';

This returns customers whose last name starts with “Sm”.

Example using a contains pattern:

    SELECT *
    FROM film
    WHERE title LIKE '%Love%';

This returns films with the word “Love” anywhere in the title.

---

## IN Operator

The `IN` operator checks whether a column value matches any value from a given list.

    SELECT *
    FROM film
    WHERE rating IN ('PG', 'PG-13', 'R');

This returns films whose rating is one of the listed values.

The `IN` operator is often more readable than using multiple `OR` conditions.

---

## WHERE Clause Placement Rule

The `WHERE` clause always appears:
- After the `FROM` clause
- Before `GROUP BY`, `HAVING`, and `ORDER BY`

General structure:

    SELECT column_name
    FROM table_name
    WHERE logical_condition;

---

## Mental Model

Logical operators act as filters applied row by row.  
Each row is tested against the condition:
- Rows that evaluate to true are included
- Rows that evaluate to false are excluded

Understanding these operators is essential for precise and effective data filtering in SQL.
