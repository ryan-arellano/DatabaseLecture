# MySQL IS NULL

## Introduction to MySQL IS NULL operator

To test whether a value is NULL or not, you use the IS NULL operator. Here’s the basic syntax of the IS NULL operator:

```sql
value IS NULL
```

If the value is NULL, the expression returns true. Otherwise, it returns false.

Note that MySQL does not have a built-in BOOLEAN type. It uses the TINYINT(1) to represent the BOOLEAN values i.e., true means 1 and false means 0.

Because the IS NULL is a comparison operator, you can use it anywhere that an operator can be used e.g., in the SELECT or WHERE clause.

See the following example:

```sql
SELECT 1 IS NULL,  -- 0
       0 IS NULL,  -- 0
       NULL IS NULL; -- 1
```

To check if a value is not NULL, you use IS NOT NULL operator:

```sql
value IS NOT NULL
```

This expression returns true (1) if the value is not NULL. Otherwise, it returns false (0).

Consider the following example:

```sql
SELECT 1 IS NOT NULL, -- 1
       0 IS NOT NULL, -- 1
       NULL IS NOT NULL; -- 0
```

## MySQL IS NULL examples

We will use the customers table in the sample database for the demonstration.

<img src="./images/customers.png" alt=""/>

The following query uses the IS NULL operator to find customers who do not have a sales representative:

```sql
SELECT
    customerName,
    country,
    salesrepemployeenumber
FROM
    customers
WHERE
    salesrepemployeenumber IS NULL
ORDER BY
    customerName;

```

This example uses the IS NOT NULL operator to get the customers who have a sales representative:

```sql
SELECT
    customerName,
    country,
    salesrepemployeenumber
FROM
    customers
WHERE
    salesrepemployeenumber IS NOT NULL
ORDER BY
   customerName;
```

## Summary

- Use the IS NULL operator to test if a value is NULL or not. The IS NULL operator returns one if a value is NULL.
- The IS NOT NULL returns one if a value is not NULL.
