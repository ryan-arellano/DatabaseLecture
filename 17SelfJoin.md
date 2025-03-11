# MySQL SELF JOIN

## Introduction to SUBTOPIC

In the previous topics, you have learned how to join a table to the other tables using INNER JOIN, LEFT JOIN, RIGHT JOIN, or CROSS JOIN clause. However, there is a special case that you need to join a table to itself, which is known as a self join.

The self join is often used to query hierarchical data or to compare a row with other rows within the same table.

To perform a self join, you must use table aliases to not repeat the same table name twice in a single query. Note that referencing a table twice or more in a query without using table aliases will cause an error.

## MySQL self join examples

Let’s take a look at the employees table in the sample database.

<img src="./images/employees.png" alt="" />

The table employees stores not only employees data but also the organization structure data. The reportsto column is used to determine the manager id of an employee.

### 1. MySQL self join using INNER JOIN clause

To get the whole organization structure, you can join the employees table to itself using the employeeNumber and reportsTo columns. The table employees has two roles: one is the Manager and the other is Direct Reports.

```sql
SELECT
    CONCAT(m.lastName, ', ', m.firstName) AS Manager,
    CONCAT(e.lastName, ', ', e.firstName) AS 'Direct report'
FROM
    employees e
INNER JOIN employees m ON
    m.employeeNumber = e.reportsTo
ORDER BY
    Manager;
```

The output only shows the employees who have a manager. However, you don’t see the President because his name is filtered out due to the INNER JOIN clause.

## 2. MySQL self join using LEFT JOIN clause

The President is the employee who does not have any manager or value in the reportsTo column is NULL .

The following statement uses the LEFT JOIN clause instead of INNER JOIN to include the President:

```sql
SELECT
    IFNULL(CONCAT(m.lastname, ', ', m.firstname),
            'Top Manager') AS 'Manager',
    CONCAT(e.lastname, ', ', e.firstname) AS 'Direct report'
FROM
    employees e
LEFT JOIN employees m ON
    m.employeeNumber = e.reportsto
ORDER BY
    manager DESC;
```

### 3. Using MySQL self join to compare successive rows

By using the MySQL self join, you can display a list of customers who locate in the same city by joining the customers table to itself.

```sql
SELECT
    c1.city,
    c1.customerName,
    c2.customerName
FROM
    customers c1
INNER JOIN customers c2 ON
    c1.city = c2.city
    AND c1.customername > c2.customerName
ORDER BY
    c1.city;
```

In this example, the table customers is joined to itself using the following join conditions:

- c1.city = c2.city makes sure that both customers have the same city.
- c.customerName > c2.customerName ensures that no same customer is included.

## SUMMARY

- Self-join is a technique in MySQL where a table is joined with itself.
- It is useful when you need to combine rows from the same table based on a relationship between columns within that table.
- Self-join allows you to compare and analyze data within a single table, such as hierarchical or recursive data structures.
