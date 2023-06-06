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

## Lab 4:
