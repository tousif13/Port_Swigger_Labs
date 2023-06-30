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

## Lab 3: 
