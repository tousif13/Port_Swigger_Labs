# DOM-based vulnerabilities

> The Document Object Model (DOM) is a web browser's hierarchical representation of the elements on the page. Websites can use JavaScript to manipulate the nodes and objects of the DOM, as well as their properties. DOM manipulation in itself is not a problem. In fact, it is an integral part of how modern websites work. However, JavaScript that handles data insecurely can enable various attacks. DOM-based vulnerabilities arise when a website contains JavaScript that takes an attacker-controllable value, known as a source, and passes it into a dangerous function, known as a sink.
 
## Lab 1: DOM XSS using web messages

This lab demonstrates a simple web message vulnerability. To solve this lab, use the exploit server to post a message to the target site that causes the print() function to be called.

### Sol :

* Notice that the home page contains an addEventListener() call that listens for a web message

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/5831a9af-0fa3-419c-87a6-810d34b56acc)

* Go to the exploit server and give the below payload

      <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">

* Deliver it to the victim.
* When the iframe loads, the `postMessage()` method sends a web message to the home page. The event listener, which is intended to serve ads, takes the content of the web message and inserts it into the `div` with the ID ads. However, in this case it inserts our `img` tag, which contains an invalid `src` attribute. This throws an error, which causes the `onerror` event handler to execute our payload
* Thus, the lab is solved.

## Lab 2: DOM XSS using web messages and a JavaScript URL

This lab demonstrates a DOM-based redirection vulnerability that is triggered by web messaging. To solve this lab, construct an HTML page on the exploit server that exploits this vulnerability and calls the print() function.

### Sol :

* Notice that the home page contains an `addEventListener()` call that listens for a web message. The JavaScript contains a flawed `indexOf()` check that looks for the strings `http:` or `https:` anywhere within the web message. It also contains the sink `location.href`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ddb0badc-b318-427a-bb0d-4a96dfee69c2)

* Go to the exploit server and add the following `iframe` to the body, remembering to replace `YOUR-LAB-ID` with your lab ID:

      <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">

* Deliver it to the victim.
* This script sends a web message containing an arbitrary JavaScript payload, along with the string `http:`. The second argument specifies that any `targetOrigin` is allowed for the web message
* When the `iframe` loads, the `postMessage()` method sends the JavaScript payload to the main page. The event listener spots the `http:` string and proceeds to send the payload to the `location.href` sink, where the `print()` function is called.
* Thus , the lab is solved.

## Lab 3: DOM XSS using web messages and JSON.parse

This lab uses web messaging and parses the message as JSON. To solve the lab, construct an HTML page on the exploit server that exploits this vulnerability and calls the print() function.

### Sol :

* Notice that the home page contains an event listener that listens for a web message. This event listener expects a string that is parsed using `JSON.parse()`. In the JavaScript, we can see that the event listener expects a `type` property and that the `load-channel` case of the `switch` statement changes the `iframe src` attribute.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/54963e74-6593-46b8-b362-f613cc2908d0)

* Go to the exploit server and add the following iframe to the body, remembering to replace YOUR-LAB-ID with your lab ID:

       <iframe src=https://YOUR-LAB-ID.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>

* Store the exploit and deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/efba68f8-491b-4109-abb7-27e085fa4a52)

* When the `iframe` we constructed loads, the `postMessage()` method sends a web message to the home page with the type `load-channel`. The event listener receives the message and parses it using `JSON.parse()` before sending it to the `switch`.
* The `switch` triggers the `load-channel` case, which assigns the `url` property of the message to the `src` attribute of the `ACMEplayer.element` `iframe`. However, in this case, the `url` property of the message actually contains our JavaScript payload.
* As the second argument specifies that any `targetOrigin` is allowed for the web message, and the event handler does not contain any form of origin check, the payload is set as the `src` of the `ACMEplayer.element` `iframe`. The `print()` function is called when the victim loads the page in their browser.
* Thus, the lab is solved.

## Lab 4: DOM-based open redirection

This lab contains a DOM-based open-redirection vulnerability. To solve this lab, exploit this vulnerability and redirect the victim to the exploit server.

### Sol :

* The blog post page contains the following link, which returns to the home page of the blog:

      <a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>

* The `url` parameter contains an open redirection vulnerability that allows you to change where the `Back to Blog` link takes the user. To solve the lab, construct and visit the following URL, remembering to change the URL to contain your lab ID and your exploit server ID:

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d9cc1b29-ec4a-45d4-bea3-017748a92356)

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/39232785-3c8e-46e7-8a4c-33403ad1243b)

* Thus, the lab is solved.

## Lab 5: DOM-based cookie manipulation

This lab demonstrates DOM-based client-side cookie manipulation. To solve this lab, inject a cookie that will cause XSS on a different page and call the print() function. You will need to use the exploit server to direct the victim to the correct pages.

### Sol :

* Notice that the home page uses a client-side cookie called lastViewedProduct, whose value is the URL of the last product page that the user visited.
* Go to the exploit server and add the following iframe to the body, remembering to replace YOUR-LAB-ID with your lab ID:

      <iframe src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">

* Store the exploit and deliver it to the victim.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/0bd658f1-8aed-477f-91eb-a834e91e11b1)

* The original source of the `iframe` matches the URL of one of the product pages, except there is a JavaScript payload added to the end. When the `iframe` loads for the first time, the browser temporarily opens the malicious URL, which is then saved as the value of the `lastViewedProduct` cookie. The `onload` event handler ensures that the victim is then immediately redirected to the home page, unaware that this manipulation ever took place. While the victim's browser has the poisoned cookie saved, loading the home page will cause the payload to execute.
* Thus, the lab is solved.

## Lab 6: Exploiting DOM clobbering to enable XSS

This lab contains a DOM-clobbering vulnerability. The comment functionality allows "safe" HTML. To solve this lab, construct an HTML injection that clobbers a variable and uses XSS to call the alert() function.

### Sol :

* The page for a specific blog post imports the JavaScript file loadCommentsWithDomPurify.js, which contains the following code:

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/71f83a02-a7f4-4a6e-ab45-c3360facd1ad)

    let defaultAvatar = window.defaultAvatar || {avatar: '/resources/images/avatarDefault.svg'}

* Go to one of the blog posts and create a comment containing the following anchors:

       <a id=defaultAvatar><a id=defaultAvatar name=avatar href="cid:&quot;onerror=alert(1)//">

* Return to the blog post and create a second comment containing any random text. The next time the page loads, the alert() is called.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/c1fc231c-4265-4e0b-94af-0bb1029955c4)

* The `defaultAvatar` object is implemented using this dangerous pattern containing the logical `OR` operator in conjunction with a global variable. This makes it vulnerable to DOM clobbering.
* You can clobber this object using anchor tags. Creating two anchors with the same ID causes them to be grouped in a DOM collection. The `name` attribute in the second anchor contains the value `avatar`, which will clobber the `avatar` property with the contents of the `href` attribute
* Notice that the site uses the DOMPurify filter in an attempt to reduce DOM-based vulnerabilities. However, DOMPurify allows you to use the `cid:` protocol, which does not URL-encode double-quotes. This means you can inject an encoded double-quote that will be decoded at runtime. As a result, the injection described above will cause the `defaultAvatar` variable to be assigned the clobbered property `{avatar: ‘cid:"onerror=alert(1)//’}` the next time the page is loaded.
* When you make a second post, the browser uses the newly-clobbered global variable, which smuggles the payload in the `onerror` event handler and triggers the `alert()`.
* Thus, the lab is solved.

## Lab 7: Clobbering DOM attributes to bypass HTML filters

This lab uses the HTMLJanitor library, which is vulnerable to DOM clobbering. To solve this lab, construct a vector that bypasses the filter and uses DOM clobbering to inject a vector that calls the print() function. You may need to use the exploit server in order to make your vector auto-execute in the victim's browser.

### Sol :

* Go to one of the blog posts and create a comment containing the following HTML:

       <form id=x tabindex=0 onfocus=print()><input id=attributes>

  ![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/e60229ed-5b83-419d-8898-a9251e441730)

* Go to the exploit server and add the following iframe to the body:

       <iframe src=https://YOUR-LAB-ID.web-security-academy.net/post?postId=3 onload="setTimeout(()=>this.src=this.src+'#x',500)">
  
* Make sure that postId is same in two payloads.
* Store the exploit and deliver it to the victim. The next time the page loads, the print() function is called.
* The library uses the `attributes` property to filter HTML `attributes`. However, it is still possible to clobber the attributes property itself, causing the length to be undefined. This allows us to inject any attributes we want into the `form` element. In this case, we use the `onfocus` attribute to smuggle the `print()` function.
* When the `iframe` is loaded, after a 500ms delay, it adds the `#x` fragment to the end of the page URL. The delay is necessary to make sure that the comment containing the injection is loaded before the JavaScript is executed. This causes the browser to focus on the element with the ID `x`, which is the form we created inside the comment. The `onfocus` event handler then calls the `print()` function.
* Thus, the lab is solved.
