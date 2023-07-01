# Cross-Site Scripting (XSS)

## Lab 1: Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function

### Sol :

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d6da919c-88b9-4b01-82ff-c0514ff2397c)

In URL, give `alert` function input as `search=<script>alert(1)</script>` to display the alert.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/fe86aca3-2bf3-4920-ba8e-248eb033f486)

Thus, lab is solved

## Lab 2: Reflected XSS into attribute with angle brackets HTML-encoded

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function.

### Sol :

First we'll look for entry point which is search box in this example.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6257437f-acd4-4490-b9a4-dba70772c902)

Submitted random alphanumeric string for reflective response

Now giving payload `"onmouseover="alert(1)` in search box to run the alert.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/9bef30ad-69f2-4647-9eb0-e0697b89ad38)

Thus, the lab is solved.

## Lab 3: Stored XSS into HTML context with nothing encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed.

### Sol :

Click on any post and we will get a comment box at end side of the page.

In comment box, give script as `<script>alert(1)</script>` to check and run the alert function.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/fb45191d-c16d-42e9-b575-ebb8a9e568cd)

It takes as comment and runs the script i.e. alert function

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/447d3c99-a617-4cfb-890f-0a776bba83fb)

Thus , lab is solved.

## Lab 4: DOM XSS in document.write sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

### Sol :

* Enter a random alphanumeric string into the search box.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/3a15f297-4c26-4ea0-85e5-5b8de28509ca)

* Right-click and inspect the element, and observe that your random string has been placed inside an img src attribute.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/eab7daaf-181d-43c0-a21e-35fc19a75ec6)

* Break out of the img attribute by searching for:

      "><svg onload=alert(1)>

* Alert is generated.
* Thus, the lab is solved.
