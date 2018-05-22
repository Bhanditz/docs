---
title: Workato connectors - SQL Server
date: 2018-02-02 06:10:00 Z
---

# SQL Server
[SQL Server](https://www.microsoft.com/en-us/sql-server/) is a relational database management system by Microsoft. It supports transactional processes, business intelligence and analytics applications for enterprises.

## Supported editions
All editions of SQL Server are supported.

## How to connect to SQL Server on Workato
The SQL Server connector uses basic authentication to authenticate with SQL Server.
![Configured SQL Server connection](/assets/images/mssql/connection.png)

<table class="unchanged rich-diff-level-one">
  <thead>
    <tr>
        <th width='25%'>Field</th>
        <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Connection name</td>
      <td>Give this SQL Server connection a unique name that identifies which SQL Server instance it is connected to.</td>
    </tr>
    <tr>
      <td>On-prem secure agent</td>
      <td>Choose an on-premise agent if your database is running in a network that does not allow direct connection. Before attempting to connect, make sure you have an active on-premise agent. Refer to the <a href="/on-prem.md">On-premise agent</a> guide for more information.</td>
    </tr>
    <tr>
      <td>Username</td>
      <td>Username to connect to SQL Server.</td>
    </tr>
    <tr>
      <td>Password</td>
      <td>Password to connect to SQL Server.</td>
    </tr>
    <tr>
      <td>Host</td>
      <td>URL of your hosted server.</td>
    </tr>
    <tr>
      <td>Port</td>
      <td>Port number that your server is running on, typically <b>3306</b>.</td>
    </tr>
    <tr>
      <td>Database</td>
      <td>Name of the SQL Server database you wish to connect to.</td>
    </tr>
    <tr>
      <td>Azure SQL</td>
      <td>Choose 'Yes' if connecting to an Azure SQL instance.</td>
    </tr>
  </tbody>
</table>

## Working with the SQL Server connector

### Table, view and stored procedure
The SQL Server connector works with all tables, views and stored procedures. These are available in pick lists in each trigger/action or you can provide the exact name.

![Table selection from pick list](/assets/images/mssql/table_pick_list.png)
*Select a table/view from pick list*

![Exact table name provided](/assets/images/mssql/table_name_text.png)
*Provide exact table/view name in a text field*

Case sensitivity of the name of a table/view depends on your database implementation. A default SQL Server is case insensitive. Databases or database objects with `CS` in the **COLLATION** indicates that it is case sensitive.

### Single row vs batch of rows
SQL Server connector can read or write to your database either as a single row or in batches. When using batch triggers/actions, you have to provide the batch size you wish to work with. The batch size can be any number between 1 and 100, with 100 being the maximum batch size.

![Batch trigger inputs](/assets/images/mssql/batch_trigger_input.png)
*Batch trigger inputs*

Besides the difference in input fields, there is also a difference between the outputs of these 2 types of operations. A trigger that processes rows one at a time will have an output datatree that allows you to map data from that single row.

![Single row output](/assets/images/mssql/single_row_trigger_output.png)
*Single row output*

However, a trigger that processes rows in batches will output them as an array of rows. The <kbd>Rows</kbd> datapill indicates that the output is a list containing data for each row in that batch.

![Batch trigger output](/assets/images/mssql/batch_trigger_output.png)
*Batch trigger output*

As a result, the output of batch triggers/actions needs to be handled differently. This [recipe](https://www.workato.com/recipes/666198) uses a batch trigger for new rows in the `users` table. The output of the trigger is used in a Salesforce bulk upsert action that requires mapping the <kbd>Rows</kbd> datapill into the source list.

![Using batch trigger output](/assets/images/mssql/using_batch_output.png)
*Using batch trigger output*

### WHERE condition
This input field is used to filter and identify rows to perform an action on. It is used in multiple triggers and actions in the following ways:
- filter rows to be picked up in triggers
- filter rows in **Select rows** action
- filter rows to be deleted in **Delete rows** action

This clause will be used as a `WHERE` statement in each request. This should follow basic SQL syntax. Refer to this [SQL Server documentation](https://docs.microsoft.com/en-us/sql/t-sql/queries/where-transact-sql) for a comprehensive list of rules for constructing `WHERE` statements.

#### Simple statements

String values must be enclosed in single quotes (`''`) and columns used must exist in the table/view.

A simple `WHERE` condition to filter rows based on values in a single column looks like this.

```sql
currency = 'USD'
```

If used in a **Select rows** action, this `WHERE` condition will return all rows that has the value 'USD' in the `currency` column. Just remember to wrap datapills with single quotes in your inputs.

![Using datapills in WHERE condition](/assets/images/mssql/use_datapill_in_where.png)
*Using datapills in `WHERE` condition*

Column names with spaces must be enclosed in double quotes (`""`) or square brackets (`[]`). For example, **currency code** must to enclosed in brackets to be used as an identifier.

```sql
[currency code] = 'USD'
```

![WHERE condition with enclosed identifier](/assets/images/mssql/where-condition-with-enclosed-identifier.png)
*`WHERE` condition with enclosed identifier*

#### Complex statements

Your `WHERE` condition can also contain subqueries. The following query can be used on the `compensation` table.

```sql
id in (select compensation_id from users where active = 0)
```

When used in a **Delete rows** action, this will delete all rows in the `compensation` table related to users who are no longer active (`active = 0`).

![Using subquery in WHERE condition](/assets/images/mssql/subquery-in-where-condition.png)
*Using subquery in WHERE condition*
