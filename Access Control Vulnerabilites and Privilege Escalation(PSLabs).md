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

## Lab 3 : 
