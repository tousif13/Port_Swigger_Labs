# Cross Site Request Forgery (CSRF)

## Lab 1: CSRF vulnerability with no defenses

This lab's email change functionality is vulnerable to CSRF.

To solve the lab, craft some HTML that uses a CSRF attack to change the viewer's email address and upload it to your exploit server.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Login with the credentials `wiener:peter` and update the email by giving new email in email box.

Find the request in the `HTTP history` of proxy.

Click `Go to exploit server` in the lab window.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/63f4ed20-c2f3-49bc-9b7d-196de1dc4761)

In body section, give following HTML code:

      <form method="POST" action="https://0a03006704daa24e806f67c500b50039.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="wiener%40web-security-academy.net">
    </form>
    <script>
        document.forms[0].submit();
    </script>

* Right-click on the request and select `Engagement tools -> Generate CSRF PoC`. Enable the option to include an `auto-submit script` and click `Regenerate`.
* Copy the HTML code and paste it into the exploit server body code.
* Change the email parameter value.
* Deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a0cfa7ea-e6c1-47ae-a2cf-98a1b7320d09)

* Thus, the lab is solved.

## Lab 2: CSRF where token validation depends on request method

This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

* Login with the credentials `wiener:peter` and update the email by giving new email in email box.
* Find the request in the `HTTP history` of proxy.
* Send the request to Burp Repeater and observe that if you change the value of the `csrf` parameter then the request is rejected.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9da1a36f-073b-470d-9d65-ce89eda421fe)

* Use `Change request method` on the context menu to convert it into a `GET` request and observe that the CSRF token is no longer verified.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/adab6b53-a37c-4ffa-abc3-fe8a0ca9bce8)

* Right-click on the request, and from the context menu select `Engagement tools -> Generate CSRF PoC`. Enable the option to include an auto-submit script and click `Regenerate`.
* Copy the HTML code and paste it into the exploit server body code.
* Change the email parameter value.
* Deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a0cfa7ea-e6c1-47ae-a2cf-98a1b7320d09)

* Thus, the lab is solved.

## Lab 3: CSRF where token validation depends on token being present

This lab's email change functionality is vulnerable to CSRF.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: `wiener:peter`

### Sol :

* Login with the credentials `wiener:peter` and update the email by giving new email in email box.
* Find the request in the `HTTP history` of proxy.
* Delete the `csrf` parameter entirely and observe that the request is now accepted

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8cb5f0ad-9fde-4f6d-ad1c-7903c83ac661)

* Right-click on the request, and from the context menu select `Engagement tools -> Generate CSRF PoC`. Enable the option to include an auto-submit script and click `Regenerate`
* Copy the HTML code and paste it into the exploit server body code.
* Change the email parameter value.
* Deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a0cfa7ea-e6c1-47ae-a2cf-98a1b7320d09)

* Thus, the lab is solved.

## Lab 4: CSRF where token is not tied to user session

This lab's email change functionality is vulnerable to CSRF. It uses tokens to try to prevent CSRF attacks, but they aren't integrated into the site's session handling system.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You have two accounts on the application that you can use to help design your attack. The credentials are as follows:

`wiener:peter`

`carlos:montoya`

### Sol :

* Open Burp's browser and log in to your account. Submit the "Update email" form, and intercept the resulting request.
* Make a note of the value of the CSRF token, then drop the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0f190bd3-cc5f-43f3-96e6-ee10fb0df9d0)

* Open a private/incognito browser window, log in to your other account, and send the update email request into Burp Repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5ffe4e23-f84c-4e84-83c9-1c34cfc17c0c)

* Observe that if we swap the CSRF token with the value from the other account, then the request is accepted.
* Create and host a proof of concept exploit as described in the solution to the CSRF vulnerability with no defenses lab. Note that the CSRF tokens are single-use, so you'll need to include a fresh one.
* Change the email parameter value.
* Deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9d6956f4-e465-4ab7-99d1-4c09614f68fc)

* Thus, the lab is solved.

## Lab 5: CSRF where token is tied to non-session cookie

This lab's email change functionality is vulnerable to CSRF. It uses tokens to try to prevent CSRF attacks, but they aren't fully integrated into the site's session handling system.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You have two accounts on the application that you can use to help design your attack. The credentials are as follows:

`wiener:peter`

`carlos:montoya`

### Sol :

* Open Burp's browser and log in to your account. Submit the "Update email" form, and find the resulting request in your Proxy history.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e4789ed9-928b-4b9f-8b4e-fd5e0786003b)

* Send the request to Burp Repeater and observe that changing the session cookie logs you out, but changing the `csrfKey` cookie merely results in the CSRF token being rejected. This suggests that the `csrfKey` cookie may not be strictly tied to the session.
* Open a private/incognito browser window, log in to your other account, and send a fresh update email request into Burp Repeater
* Observe that if we swap the `csrfKey` cookie and `csrf` parameter from the first account to the second account, the request is accepted.
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9cfe0cb0-da00-46bc-a31e-d6ee3e8e6659)

* Close the Repeater tab and incognito browser.
* Back in the original browser, perform a search, send the resulting request to Burp Repeater, and observe that the search term gets reflected in the Set-Cookie header. Since the search function has no CSRF protection, you can use this to inject cookies into the victim user's browser.
* Create and host a proof of concept exploit as described in the solution to the CSRF vulnerability with no defenses lab, ensuring that you include your CSRF token. The exploit should be created from the email change request
* Remove the auto-submit <script> block, and instead add the following code to inject the cookie by replacing the Lab ID and CSRF Key

      <img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None" onerror="document.forms[0].submit()">

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/be8a3130-1a69-4a03-b3b3-083454c202f4)

* Change the email address in your exploit so that it doesn't match your own.
* Store the exploit, then click `Deliver to victim` to solve the lab.

## Lab 6: CSRF where token is duplicated in cookie

This lab's email change functionality is vulnerable to CSRF. It attempts to use the insecure "double submit" CSRF prevention technique.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

