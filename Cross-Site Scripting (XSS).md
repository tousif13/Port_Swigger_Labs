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
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/819eb6ca-4eb7-4acf-b266-883cd98b57c0)

* Alert is generated.
* Thus, the lab is solved.

## Lab 5: DOM XSS in document.write sink using source location.search inside a select element

This lab contains a DOM-based cross-site scripting vulnerability in the stock checker functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search which you can control using the website URL. The data is enclosed within a select element.

To solve this lab, perform a cross-site scripting attack that breaks out of the select element and calls the alert function.

### Sol :

* On the product pages, notice that the dangerous JavaScript extracts a `storeId` parameter from the `location.search` source. It then uses `document.write` to create a new option in the select element for the stock checker functionality.
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8df68d39-7c7c-47cd-8752-08a0ab072c1a)

* Add a `storeId` query parameter to the URL and enter a random alphanumeric string as its value. Request this modified URL.
* In the browser, notice that your random string is now listed as one of the options in the drop-down list.
* Right-click and inspect the drop-down list to confirm that the value of your storeId parameter has been placed inside a select element
* Change the URL to include a suitable XSS payload inside the storeId parameter as follows:

        product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1df96d5d-2e05-4bfb-8ab6-2948ad55c55b)

* Thus, the alert is generated and lab is solved.

## Lab 6: DOM XSS in innerHTML sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

### Sol :

* Enter the following into the into the search box:

        <img src=1 onerror=alert(1)>

* Click Search

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/2850c3c7-3b07-470d-b5bb-0e4365cf2d17)

* The value of the `src` attribute is invalid and throws an error. This triggers the `onerror` event handler, which then calls the `alert()` function. As a result, the payload is executed whenever the user's browser attempts to load the page containing your malicious post.
* Thus, the lab is solved.

## Lab 7: DOM XSS in jQuery anchor href attribute sink using location.search source

This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie.

### Sol :

* On the Submit feedback page, change the query parameter `returnPath` to `/` followed by a random alphanumeric string.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/33ebbff8-595e-4719-9048-3817408ed3d7)

* Right-click and inspect the element, and observe that your random string has been placed inside an a href attribute.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/32aec7a5-a915-4c51-b5b4-da007abe9ea8)

* Change returnPath to:

        javascript:alert(document.cookie)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/8a1d5e29-d6f7-4b11-a59a-dc7032d795b0)

* Click Back, we will get the alert box and lab is solved.

## Lab 8: DOM XSS in jQuery selector sink using a hashchange event

This lab contains a DOM-based cross-site scripting vulnerability on the home page. It uses jQuery's $() selector function to auto-scroll to a given post, whose title is passed via the location.hash property.

To solve the lab, deliver an exploit to the victim that calls the print() function in their browser.

### Sol :

* Notice the vulnerable code on the home page using Burp or the browser's DevTools
* From the lab banner, open the exploit server
* In the Body section, add the following malicious iframe:

        <iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>

* Store the exploit, then click View exploit to confirm that the print() function is called

  ![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/55eb2b3c-3861-494c-98e7-c2fa4114ec90)

* Go back to the exploit server and click Deliver to victim to solve the lab.

## Lab 9: Stored XSS into anchor href attribute with double quotes HTML-encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality. To solve this lab, submit a comment that calls the alert function when the comment author name is clicked

### Sol :

