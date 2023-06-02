# Access control vulnerabilities and privilege escalation

## Lab 1: Unprotected admin functionality

This lab has an unprotected admin panel.

Solve the lab by deleting the user carlos.

### Sol :

In the lab URL, give `/robots.txt` to check the allow or disallowance.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/da37564c-3d5b-45ae-a94f-7191924668da)

As it shown disallowance, it exposed `administartor-panel` admin's control. Give `/administartor-panel` in URL.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/770d79e6-a5db-47f4-8bed-42f5feef3c62)

We can see the Users and we can delete them. Delete `carlos` user to solve the lab.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d180b946-51e6-481b-bf65-52f41f70a77f)

The user is deleted successfully and lab is solved.

## Lab 2 : Unprotected admin functionality with unpredictable URL

This lab has an unprotected admin panel. It's located at an unpredictable location, but the location is disclosed somewhere in the application.

Solve the lab by accessing the admin panel, and using it to delete the user carlos.

### Sol :

If we see the page source of the lab. we will find a Javascript code that discloses admin's panel.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b53c34dd-0930-4114-be99-3361578ac0eb)

As we can see admin's panel got exposed which is `/admin-uiey69`

Append the admin's panel in the URL. We will get the administrator controls.

Delete the `carlos` user to complete the lab.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/71d1b5c2-9809-45aa-8f9a-a75cca287bf9)

User is deleted successfully and lab is solved.

## Lab 3 : User role controlled by request parameter

This lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

If we give `/admin` in URL, we will be restricted to access.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/32081d6c-9222-41e1-8a70-51291289effa)

We will intercept the request of login form.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9dc49d52-3abe-44d9-9ce8-577335587151)

Forward the request we will get the `Admin=false` in Cookie section

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/239d765d-e157-4180-acd0-f262578e3219)

Set the Admin value as true i.e. `Admin=true` in Cookie section

Forward the request we will get the admin panel.

Click delete `carlos` user and set `Admin=True` and click forward

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/826462f4-5b80-481c-9f22-650a43e58677)

After the request is forwarded, user gets deleted and lab is solved.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/869fcbd2-e3f4-4ffb-ab53-41538b08edea)

## Lab 4 : User role can be modified in user profile

This lab has an admin panel at /admin. It's only accessible to logged-in users with a roleid of 2.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Login as `wiener:peter` credentials and give update email in the provided box.

Intercept with Proxy and send it to the Repeater

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/84265e27-8a53-4fd3-84c1-e18d58f3b062)

As you can see, we will get `roleid:1` in response. give `roleid:2` in request to get admin's access.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6492e934-d030-42a5-8c41-f5b92c10620f)

Go to admin's panel and delete the carlos user.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/eca0bb29-ed60-4f24-8d97-fc9503586a02)

User deleted successfully and lab got solved.

## Lab 5 : URL-based access control can be circumvented

This website has an unauthenticated admin panel at /admin, but a front-end system has been configured to block external access to that path. However, the back-end application is built on a framework that supports the X-Original-URL header.

To solve the lab, access the admin panel and delete the user carlos.

### Sol :

If we try `/admin` we won't get the access.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/436d5861-a211-44eb-a949-588a29fda9fd)

We will intercept the request and send it to repeater

We will add new HTTP header : `X-Original-Url: /invalid` and we will see `Not Found` output

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6f5c9a26-d023-4894-b682-83e6146c355f)

If we give `X-Original-Url: /admin` then we can get the access of admin panel.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8ee41ddf-4586-49f5-b41a-0fb1da463495)

Give `/?username=carlos` in GET request.

Give `X-Original-URL: /admin/delete` HTTP header to delete the carlos user

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d9bd0752-847d-4833-98ea-faa21ad1dcc2)

User got deleted and lab is solved.

## Lab 6 : Method-based access control can be circumvented

This lab implements access controls based partly on the HTTP method of requests. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.

### Sol :

Login admin panel by using credentials : `administrator:admin`

Go to admin panel and upgrade the carlos user. Intercept the request and send it to repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6de530b6-c597-4797-b07a-7247168942d3)

Open another incognito window and open the lab with `wiener:peter` credentials.

Intercept the request and send it to repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/18b29841-9d91-4a8c-ba2d-128cca7de149)

Attempt to re-promote carlos user using `wiener's` cookie ID and paste it in Admin's cookie ID field. We will get `Unauthorized` message

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/c878f71d-c11a-4114-9477-67a2e5f1465d)

After changing `POST` to `POSTX`. If we run again then we will get `Missing parameter` message.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/58d6ab48-f37c-4164-bd38-0322716deff4)

Right click and choose `change request method` and give `wiener` as username and change Cookie ID of to `wiener's` one.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/12b0b6ee-c9b7-4f2d-af97-0d897309ecbd)

Thus, lab got solved.

## Lab 7 : User ID controlled by request parameter

This lab has a horizontal privilege escalation vulnerability on the user account page.

To solve the lab, obtain the API key for the user carlos and submit it as the solution.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Login with the `wiener:peter` credentials.

Intercept the request in Burpsuite after clicking `My Account`.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/4c8449b3-ae76-4686-96d1-b87331a758ff)

We got the id of wiener and replace the id of wiener to carlos and forward the request.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/c2e02cb0-ee6f-4078-b93b-8a9309fb9ddd)

We got the carlos API key. Submit it and the lab is solved.

## Lab 8 : User ID controlled by request parameter, with unpredictable user IDs

This lab has a horizontal privilege escalation vulnerability on the user account page, but identifies users with GUIDs.

To solve the lab, find the GUID for carlos, then submit his API key as the solution.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Find a blog post by carlos

By clicking on carlos user, we will get the User ID of carlos

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/798a5707-661a-48e2-937d-09f75d2d540a)

Login using `wiener:peter` credentials and intercept the request of `My Account` action.

We will get the User ID of wiener's. Replace the value of carlos and forward the request. We will get carlos user.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/f3efb396-8093-4de1-8d33-bf766085fc60)

Submit the carlo's API key and solved the lab.

## Lab 9 : User ID controlled by request parameter with data leakage in redirect

This lab contains an access control vulnerability where sensitive information is leaked in the body of a redirect response.

To solve the lab, obtain the API key for the user carlos and submit it as the solution.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Login with the `wiener:peter` credentials.

Intercept the request and send it to repeater

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/98ce1ae5-f76b-419c-b219-1550690c346c)

We can see the API key and replace the wiener user with carlos and see the response.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/159d1d61-da5d-40cc-a4d8-9c4d71d46259)

We got the API key of the user `carlos` and submit to solve the lab.

## Lab 10 : User ID controlled by request parameter with password disclosure

This lab has user account page that contains the current user's existing password, prefilled in a masked input.

To solve the lab, retrieve the administrator's password, then use it to delete carlos.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Login with the `wiener:peter` credentials.

Intercept the request by burpsuite to know the ID. Change the id to `administrator` and run it for the response.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d4f44412-966b-4fd6-b464-8a33ea3e50f6)

We can see the admin's password from the response.

Login with the administrator account

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d7edb283-4b84-49ba-97eb-33dda6a64ac5)

Delete the carlos user and solved the lab.

## Lab 11 : Insecure direct object references

This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.

Solve the lab by finding the password for the user carlos, and logging into their account

### Sol :

Click `Live Chat` and send a message.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/64da36b3-b7ab-4fcb-b360-05f6764ccbf8)

Intercept the requests of `View Transcript`

Forward the requests of Transcript untill transcript file is visible.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8b50b89a-49a0-4676-af10-6556d271f143)

After changing the transcript file as `transcript/1.txt`, forward the request. `1.txt` file is downloaded.

Open the file and we can see the password.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/7529bb2b-54fc-4096-94bc-011aff68c1c3)

Login with the carlos and the retreived password credentials.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ea221490-7c37-4d8c-b511-e970b8a0633f)

Thus, the lab is solved.

## Lab 12 : Multi-step process with no access control on one step

This lab has an admin panel with a flawed multi-step process for changing a user's role. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.

### Sol :

