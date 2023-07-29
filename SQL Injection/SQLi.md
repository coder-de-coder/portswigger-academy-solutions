# SQL Injection LAbs

## APPRENTICE

### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
SQL vuln in the address bar.
Payload →
```
web-security-academy.net/filter?category=Gifts' OR 1=1 --
```

### SQL injection vulnerability allowing login bypass
- The SQL injection vuln in the username field can be detected by entering a single coma (’) into it which produces a server side error on the application.
- This error is because we have disturbed the functionality of the sql query.

Payload →

Enter administrator’-- in the username filed and any random password.



Alternatevely, you can use burpsuit to intercept the request as well.

## PRACTITIONER  

### SQL injection UNION attack, determining the number of columns returned by the query

There is a chance of SQL Union injection in the url as the application is rendering a table of information which is probalby from a particular table in the db.

For SQL Union attack, the correct number of entries being requested in the orignal query is to be determined by the trial and error method which in this case is 3.
```
web-security-academy.net/filter?category=Accessories%27%20UNION%20SELECT%20NULL,NULL,NULL--
```