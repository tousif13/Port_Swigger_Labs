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

Thus, lab is solved.

## Lab 2: Username enumeration via subtly different responses

This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the given wordlists.

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

### Sol :

Give any username and password. It will give invalid response.

Intercept the request at `HTTP History` in Proxy.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d566e218-cdb0-4215-ba7e-6658be813e49)

Send it to the `Intruder`

Give payload symbol for username only for now.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/85997ddb-c27f-4c45-b08a-dac75045435c)

Under options, in `Grep-Extract` field. add `Invalid Username or Password` attribute to it.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5b569992-fe45-476f-9d09-1e8130dcc49f)

Start the attack by clicking `start attack`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e210a87c-4b2c-483d-a17f-2f2cec2052be)

We can see the `alterwind` username has hit the payload as we got unusual pattern than others that it doesn't have `.` at end.

Now clear the payload symbol of username and add it to the password field. The username we identified should be given in the username field. In payload options, use the passwords given in `Canditate Passwords` list.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/37cac23b-fb0c-4e2b-8523-c2a81ec69585)

Start the attack

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ed95f864-c583-4d55-a026-a05d457df88e)

In the above image, we got the response code as `302` which is matching password for the username given and others give response code as `200`. 

Login with the found out username and password. ( Username `alterwind` Password `1234567` in this case)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d4bed274-d4d9-4968-bde0-853438982d6b)

Thus, lab is solved.

## Lab : 2FA simple bypass

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.

### Sol :

We're given with our own credentials. Login with the `wiener:peter`

Click the `Email client` button to access the emails. You will get the security code in email.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0c13b3cf-87b2-4203-bf15-23572d18166a)

Submit the security code. we will get `/my-account` page in URL.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/51fa11ee-edb0-4402-89ae-ef9a543153e4)

Log out of `wiener's` account.

Login using victim's credentials (`carlos:montoya`).

To bypass 2FA, give `/my-account` in URL. This will bypass the 2FA of carlos.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1d15aa62-da7b-4327-9bd3-d8f760cb0d03)

Thus, lab is solved.

## Lab : 
