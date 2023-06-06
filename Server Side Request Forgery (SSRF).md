# Server Side Request Forgery (SSRF)

## Lab 1: Basic SSRF against the local server

Try by adding `/admin` in the URL as it doesn't allow access for admin's page.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6f1962d1-5771-4484-a46e-3d7d1cbbe8ea)

Check stock of any product and intercept the request from the proxy.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a38993bd-7183-4856-9620-d73969814389)

In the `stockApi` parameter give `http://localhost/admin` payload to access the admin's panel.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9fe974df-0091-493d-b6a5-ac706833dcfa)

Find the `/admin/delete?username=carlos` at response side and give it to the `stockApi` parameter and run it.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/47e43736-15a7-497e-9fa8-39402e0a3712)

This will delete the user `carlos` as the request done through localhost and the lab is solved.

## Lab 2: 
