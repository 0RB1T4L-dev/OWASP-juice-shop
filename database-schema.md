## Database Schema
**Category:** Injection

**Difficulty:** 3/6

**Challenge Description:** Exfiltrate the entire DB schema definition via SQL Injection.

## Approach

### 1. Building up on former Achievements

We already know from [Login Admin](/login-admin.md) that the email input field from the login form is vulnerable to SQL injection.

### 2. Getting the Database Schema

With the help of the [PayloadsAllTheThings Repository](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md) it was easy to make query which retrieves the databse schema.

My first query looked like this: **' UNION SELECT sql FROM sqlite_master --**, but I got an error saying "SELECTs to the left and right of UNION do not have the same number of result columns" as response.

After some searching, I found out, that you can just add a null column to the SELECT statement which is missing columns. After that I tried it with different amount of null columns, but as soon as I reached 12 null columns, the SQL injection stopped working.

### 3. Finding another vulnerable Input Field

After some time I got frustrated and decided to try other input fields. I didn't have to look very long, because the search input is also vulnerable to SQL injection.

![Search SQLi Proof](/images/search-sqli-proof.png)

I tried the same approach and got further as with the other SQL injection, but ultimately ran into another error.

![Error with NULL columns](/images/sqli-union-null-columns.png)

After some research I found another way to fix the unequal column count ([sqlinjection.net/union/](https://www.sqlinjection.net/union/)).

With that, it finally worked I was able to craft a working SQL query: **a'))+UNION+SELECT+sql,2,3,4,5,6,7,8,9+FROM+sqlite_master--**

![Extracted Database Schema](/images/database-schema.png)