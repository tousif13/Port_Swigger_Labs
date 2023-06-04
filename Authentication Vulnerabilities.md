# Authentication Vulnerabilities

## Lab 1: Username enumeration via different responses

This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the wordlists

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

### Sol :

Give any username and password. It will give invalid response.

Intercept the request at `HTTP History` in Proxy.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b908d504-3acc-49b4-8076-8b23c44e4a36)

Send it to the `Intruder`

Give payload symbol for username only for now.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/85997ddb-c27f-4c45-b08a-dac75045435c)

Under payload options, Select `simple list` and paste the usernames list of given `Canditate Usernames` list.

Click `start attack`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/f460b621-fd67-436a-9e79-dcb481bfa0af)

In the above image, we can see the `ec2-user` has a greater length than others. This is the username we're looking for because it's response says `Incorrect Password` while for others it says `Invalid Username`.

Now clear the payload symbol of username and add it to the password field. The username we identified should be given in the username field. In payload options, use the passwords given in `Canditate Passwords` list.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/35263d46-4476-4cd1-b02c-eecf078a3dd1)

click `start attack`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/efa1cbb3-7d81-4e12-bbf7-56ead635414b)

In the above image, we got the response code as `302` which is matching password for the username given and others give response code as `200`. 

Login with the found out username and password. ( Username `ec2-user` Password `11111111` in this case)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/47ee3f19-df59-494e-9f31-f8223342aa95)
