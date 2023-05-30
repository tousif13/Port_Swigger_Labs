# Cross-Site Scripting (XSS)

## Lab: Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function

### Sol :

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d6da919c-88b9-4b01-82ff-c0514ff2397c)

In URL, give `alert` function input as `search=<script>alert(1)</script>` to display the alert.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/fe86aca3-2bf3-4920-ba8e-248eb033f486)

Thus, lab is solved

## Lab: Reflected XSS into attribute with angle brackets HTML-encoded

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function.

