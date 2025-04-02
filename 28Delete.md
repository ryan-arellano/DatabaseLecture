# MySQL DELETE

## Introduction to MySQL DELETE statement

To delete data from a table, you use the MySQL DELETE statement. The following illustrates the syntax of the DELETE statement:

```sql
DELETE FROM table_name
WHERE condition;
```

In this statement:

- First, specify the table from which you delete data.
- Second, use a condition to specify which rows to delete in the WHERE clause. The DELETE statement will delete rows that match the condition,

Notice that the WHERE clause is optional. If you omit the WHERE clause, the DELETE statement will delete all rows in the table.

Besides deleting data from a table, the DELETE statement returns the number of deleted rows.

To delete data from multiple tables using a single DELETE statement, you use the DELETE JOIN statement which will be covered in the next tutorial.

To delete all rows in a table without the need of knowing how many rows deleted, you should use the TRUNCATE TABLE statement to get better performance.

For a table that has a foreign key constraint, when you delete rows from the parent table, the rows in the child table will be deleted automatically by using the ON DELETE CASCADE option.

## MySQL DELETE examples

We will use the employees table in the sample database for the demonstration.

<img src="./images/employees.png" alt="" />

---

**_NOTE:_** Once you delete data, it is gone. Later, you will learn how to put the DELETE statement in a transaction so that you can roll it back.

---

Suppose you want to delete employees whose the officeNumber is 4, you use the DELETE statement with the WHERE clause as shown in the following query:

```sql
DELETE FROM employees
WHERE officeCode = 4;
```

To delete all rows from the employees table, you use the DELETE statement without the WHERE clause as follows:

```sql
DELETE FROM employees;
```

All rows in the employees table deleted.

## MySQL DELETE and LIMIT clause

If you want to limit the number of rows to delete, use the LIMIT clause as follows:

```sql
DELETE FROM table_table
LIMIT row_count;
```

Note that the order of rows in a table is unspecified, therefore, when you use the LIMIT clause, you should always use the ORDER BY clause.

```sql
DELETE FROM table_name
ORDER BY c1, c2, ...
LIMIT row_count;
```

Consider the following customers table in the sample database:

<img src="./images/customers.png" alt="" />

For example, the following statement sorts customers by customer names alphabetically and deletes the first 10 customers:

```sql
DELETE FROM customers
ORDER BY customerName
LIMIT 10;
```

Similarly, the following DELETE statement selects customers in France, sorts them by credit limit in from low to high, and deletes the first 5 customers:

```sql
DELETE FROM customers
WHERE country = 'France'
ORDER BY creditLimit
LIMIT 5;
```

## Summary

- The DELETE statement in MySQL is used to remove records from a table based on specified conditions.
- It allows you to use the WHERE clause to specify the conditions for deleting specific rows.
- The DELETE statement can be used to delete individual rows or multiple rows at once, depending on the conditions specified.
