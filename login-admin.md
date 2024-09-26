## Login Admin
**Category:** Injection

**Difficulty:** 2/6

**Challenge Description:** Log in with the administrator's user account.

## Approach

### 1. Login Form is vulnerable to SQL Injection

As the name and the category of the challenge already suggests, the login form is vulnerable to SQL Injection, which can be easily be confirmed.

![SQL Injection in Browser](/images/webform-sql-injection.png)

*[object Object]* sounds interesting, but we need a little bit more information, time to start BurpSuite.

In BurpSuite, we get a more detailed view of the response and it's also much easier to play around with the input.

![SQL Injection in BurpSuite](/images/sql-injection-proof-burp.png)

As you can see in the image above, we even get the fully SQL query, which was used. And we also see that SQLite is used.

### 2. Getting the Email of the Administrator

I already found the email before by accident. The review section of products shows the email of the use who wrote the review. The review for the first product in the shop, was written by the administrator.

![Admin Email](/images/admin-email.png)

### 3. Bypass the Login Check

After getting the email of the admin, it's time make use of this vulnerability and login as the administrator.

The SQL query used in the login process is:

```SQL
SELECT * FROM Users WHERE email = '<email>' AND password = '<hash of password>' AND deletedAt IS NULL
```

Because the hash of the password is generated before the query, only the email input field is vulnerable.

The input for the email field in order to login as the administrator is the following: **admin@juice-sh.op' AND (password='xx' OR True) --**