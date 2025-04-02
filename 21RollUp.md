# MySQL ROLLUP

## Setting up a sample table

The following statement creates a new table named sales that stores the order values summarized by product lines and years. The data comes from the products, orders, and orderDetails tables in the sample database.

```sql
CREATE TABLE sales
SELECT
    productLine,
    YEAR(orderDate) orderYear,
    SUM(quantityOrdered * priceEach) orderValue
FROM
    orderDetails
        INNER JOIN
    orders USING (orderNumber)
        INNER JOIN
    products USING (productCode)
GROUP BY
    productLine ,
    YEAR(orderDate);

```

The following query returns all rows from the sales table:

```sql
SELECT * FROM sales;
```

## MySQL ROLLUP Overview

A grouping set is a set of columns to which you want to group. For example, the following query creates a grouping set denoted by (productline)

```sql
SELECT
    productline,
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY
    productline;
```

The following query creates an empty grouping set denoted by the ():

```sql
SELECT
    SUM(orderValue) totalOrderValue
FROM
    sales;
```

If you want to generate two or more grouping sets together in one query, you may use the UNION ALL operator as follows:

```sql
SELECT
    productline,
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY
    productline
UNION ALL
SELECT
    NULL,
    SUM(orderValue) totalOrderValue
FROM
    sales;
```

Because the UNION ALL requires all queries to have the same number of columns, we added NULL in the select list of the second query to fulfill this requirement.

The NULL in the productLine column identifies the grand total super-aggregate line.

This query is able to generate the total order values by product lines and also the grand total row. However, it has two problems:

1. The query is quite lengthy.
2. The performance of the query may not be good since the database engine has to internally execute two separate queries and combine the result sets into one.

To fix these issues, you can use the ROLLUP clause.

The ROLLUP clause is an extension of the GROUP BY clause with the following syntax:

```sql
SELECT
    select_list
FROM
    table_name
GROUP BY
    c1, c2, c3 WITH ROLLUP;

```

As clearly shown in the output, the ROLLUP clause generates not only the subtotals but also the grand total of the order values.

If you have more than one column specified in the GROUP BY clause, the ROLLUP clause assumes a hierarchy among the input columns.

For example:

```sql
GROUP BY c1, c2, c3 WITH ROLLUP
```

The ROLLUP assumes that there is the following hierarchy:

```sql
c1 > c2 > c3
```

And it generates the following grouping sets:

```sql
(c1, c2, c3)
(c1, c2)
(c1)
()
```

And in case you have two columns specified in the GROUP BY clause:

```sql
GROUP BY c1, c2 WITH ROLLUP
```

then the ROLLUP generates the following grouping sets:

```sql
(c1, c2)
(c1)
()
```

See the following query example:

```sql
SELECT
    productLine,
    orderYear,
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY
    productline,
    orderYear
WITH ROLLUP;
```

The ROLLUP generates the subtotal row every time the product line changes and the grand total at the end of the result.

The hierarchy in this case is:

```sql
productLine > orderYear
```

If you reverse the hierarchy, for example:

```sql
SELECT
    orderYear,
    productLine,
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY
    orderYear,
    productline
WITH ROLLUP;
```

The ROLLUP generates the subtotal every time the year changes and the grand total at the end of the result set.

The hierarchy in this example is:

```sql
orderYear > productLine
```

## SUMMARY

- ROLLUP is an extension of the GROUP BY clause in MySQL that allows you to generate subtotals and grand totals in query results.
- It creates grouping sets based on the hierarchy specified in the GROUP BY clause, generating subtotals for each level of the hierarchy and a grand total row.
- ROLLUP simplifies the query and improves performance compared to using separate queries with UNION ALL, as it generates the subtotals and grand total in a single query execution.
