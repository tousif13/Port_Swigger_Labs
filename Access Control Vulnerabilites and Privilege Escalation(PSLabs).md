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
