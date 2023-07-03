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

* Post a comment with a random alphanumeric string in the "Website" input, then use Burp Suite to intercept the request and send it to Burp Repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/b0bb5f34-c613-4b0c-aa98-cb957ef8ee85)

* Make a second request in the browser to view the post and use Burp Suite to intercept the request and send it to Burp Repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/2f22e898-f3bb-4db6-9998-d9512b32e124)

* Observe that the random string in the second Repeater tab has been reflected inside an anchor href attribute.
* Repeat the process again but this time replace your input with the following payload to inject a JavaScript URL that calls alert:

      javascript:alert(1)
* Verify the technique worked by right-clicking, selecting "Copy URL", and pasting the URL in the browser. Clicking the name above your comment should trigger an alert.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/238e675b-7765-4ddc-a5bb-bab5c8183876)

* Thus, the lab is solved.

## Lab 10: Reflected XSS into a JavaScript string with angle brackets HTML encoded

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function.

### Sol :

* Submit a random alphanumeric string in the search box, then use Burp Suite to intercept the search request and send it to Burp Repeater.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/275ef446-fd7b-491c-a50d-1636e5ebd02a)

* Observe that the random string has been reflected inside a JavaScript string.
* Replace your input with the following payload to break out of the JavaScript string and inject an alert:

      '-alert(1)-'

* Verify the technique worked by right clicking, selecting "Copy URL", and pasting the URL in the browser. When you load the page it should trigger an alert.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/12c93a7b-c9fd-426e-803a-961080f023dd)

* Thus, the lab is solved.

## Lab 11: DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

This lab contains a DOM-based cross-site scripting vulnerability in a AngularJS expression within the search functionality.

AngularJS is a popular JavaScript library, which scans the contents of HTML nodes containing the ng-app attribute (also known as an AngularJS directive). When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces. This technique is useful when angle brackets are being encoded.

To solve this lab, perform a cross-site scripting attack that executes an AngularJS expression and calls the alert function.

### Sol :

* Enter a random alphanumeric string into the search box.
* View the page source and observe that your random string is enclosed in an ng-app directive.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/84db6cd0-4fff-4dc2-a334-df42c37c1bb5)

* Enter the following AngularJS expression in the search box:

        {{$on.constructor('alert(1)')()}}
* Click search.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a5ed1e4a-5c17-46e0-9fd5-70ff456b4e2e)

* Alert is triggered and lab is solved.

## Lab 12: Reflected DOM XSS

This lab demonstrates a reflected DOM vulnerability. Reflected DOM vulnerabilities occur when the server-side application processes data from a request and echoes the data in the response. A script on the page then processes the reflected data in an unsafe way, ultimately writing it to a dangerous sink.

To solve this lab, create an injection that calls the alert() function

### Sol :

* In Burp Suite, go to the Proxy tool and make sure that the Intercept feature is switched on.
* Back in the lab, go to the target website and use the search bar to search for a random test string, such as "XSS".
* Return to the Proxy tool in Burp Suite and forward the request.
* On the Intercept tab, notice that the string is reflected in a JSON response called `search-results`.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/c41c70b8-f97d-486f-9a4e-61be242e18c1)

* From the Site Map, open the searchResults.js file and notice that the JSON response is used with an eval() function call.
* By experimenting with different search strings, you can identify that the JSON response is escaping quotation marks. However, backslash is not being escaped.
* To solve this lab, enter the following search term:

      \"-alert(1)}//

> As you have injected a backslash and the site isn't escaping them, when the JSON response attempts to escape the opening double-quotes character, it adds a second backslash. The resulting double-backslash causes the escaping to be effectively canceled out. This means that the double-quotes are processed unescaped, which closes the string that should contain the search term.

* An arithmetic operator (in this case the subtraction operator) is then used to separate the expressions before the alert() function is called. Finally, a closing curly bracket and two forward slashes close the JSON object early and comment out what would have been the rest of the object. As a result, the response is generated as follows:

      {"searchTerm":"\\"-alert(1)}//", "results":[]}

* Thus, the lab is solved.

## Lab 13: Stored DOM XSS

This lab demonstrates a stored DOM vulnerability in the blog comment functionality. To solve this lab, exploit this vulnerability to call the alert() function.

### Sol :

* Post a comment containing the following vector:

      <><img src=1 onerror=alert(1)>
* In an attempt to prevent XSS, the website uses the JavaScript replace() function to encode angle brackets. However, when the first argument is a string, the function only replaces the first occurrence. We exploit this vulnerability by simply including an extra set of angle brackets at the beginning of the comment. These angle brackets will be encoded, but any subsequent angle brackets will be unaffected, enabling us to effectively bypass the filter and inject HTML.
* Thus, the lab is solved.

## Lab 14: Reflected XSS into HTML context with all tags blocked except custom ones

This lab blocks all HTML tags except custom ones.

To solve the lab, perform a cross-site scripting attack that injects a custom tag and automatically alerts document.cookie.

### Sol :

* Go to the exploit server and paste the following code, replacing YOUR-LAB-ID with your lab ID:

        <script>
      location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
      </script>

* Click "Store" and "Deliver exploit to victim"
  
![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/510361b9-a53a-49f5-ba6c-a49e8cd14bb3)

* This injection creates a custom tag with the ID x, which contains an onfocus event handler that triggers the alert function. The hash at the end of the URL focuses on this element as soon as the page is loaded, causing the alert payload to be called
* Thus, the lab is solved.

## Lab 15: Reflected XSS in canonical link tag

This lab reflects user input in a canonical link tag and escapes angle brackets.

To solve the lab, perform a cross-site scripting attack on the home page that injects an attribute that calls the alert function.

To assist with your exploit, you can assume that the simulated user will press the following key combinations:

      ALT+SHIFT+X
      CTRL+ALT+X
      Alt+X
Please note that the intended solution to this lab is only possible in Chrome.

### Sol :

* Visit the following URL, replacing YOUR-LAB-ID with your lab ID:

        https://YOUR-LAB-ID.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)
* This sets the `X` key as an access key for the whole page. When a user presses the access key, the `alert` function is called
* To trigger the exploit on yourself, press one of the following key combinations:

        On Windows: ALT+SHIFT+X
      On MacOS: CTRL+ALT+X
      On Linux: Alt+X
* Thus, the lab is solved.
