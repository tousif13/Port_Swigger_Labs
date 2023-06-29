# OS Command Injection
> OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data. Very often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, exploiting trust relationships to pivot the attack to other systems within the organization.

## Lab 1 : OS command injection, simple case

This lab contains an OS command injection vulnerability in the product stock checker.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the whoami command to determine the name of the current user.

### Sol :

If we click a product and click check stock. then, we will get the ProductId & StoreId

![image](https://user-images.githubusercontent.com/33444140/236799000-1d2eeac3-d715-48e9-ae44-6bfa91b84c9f.png)

Then, we are giving `|whoami` to know the `Username` 
    
     ProductId=1&StoreId=1|whoami

We got the username and the lab is solved

 ![Screenshot 2023-05-08 151841](https://user-images.githubusercontent.com/33444140/236875721-9bf7a4ac-7f9a-4939-a6b7-e63e5292c5dc.png)
 
## Lab 2 : Blind OS command injection with time delays

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response.

To solve the lab, exploit the blind OS command injection vulnerability to cause a 10 second delay.
    
### Sol :

* Go to the submit feedback option and give a feedback
* Intercept the connection, then you will see the name, email, subject, message fields
![image](https://user-images.githubusercontent.com/33444140/236868717-e1624987-d07c-4e0e-82d2-54f00405a510.png)

* Here email is the vulnerable one and we will exploit this with time delays
* In email field give command

      email=test||ping+-c+10+127.0.0.1||
      
![image](https://user-images.githubusercontent.com/33444140/236869514-9ddd8a6a-b1cf-459f-8acd-cf2a56154081.png)

* If we run this request the page gets delayed to its loopback address and the lab is solved

## Lab 3 : Blind OS command injection with output redirection

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at:

/var/www/images/
The application serves the images for the product catalog from this location. You can redirect the output from the injected command to a file in this folder, and then use the image loading URL to retrieve the contents of the file.

To solve the lab, execute the whoami command and retrieve the output

### Sol :

Intercepting the request 

![3rdlab](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/f8a310d1-39f4-4d3b-972f-9f810e8f9371)

![3rdlab2](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/94944035-f8a0-42c2-9998-6e51d2f521f2)

We will redirect the output to a example file by using command

        email=||whoami>/var/www/images/example.txt||

![3rdlab4](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a341e796-b6f7-49c2-a639-527fab69c0e2)

![3rdlab5](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1722c173-2af3-4f76-a771-b5c0f84d3d26)

## Lab 4: Blind OS command injection with out-of-band interaction

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, exploit the blind OS command injection vulnerability to issue a DNS lookup to Burp Collaborator.

### Sol :

* Use Burp Suite to intercept and modify the request that submits feedback.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/122d8b1c-416f-4bf7-a54c-3deb765023da)

* Modify the `email` parameter, changing it to below payload by giving your own burp collaborator domain

      email=x||nslookup+x.BURP-COLLABORATOR-SUBDOMAIN||

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/4e958d39-fa2e-4126-99ca-4b9e5a5980f4)

* Thus, the lab is solved.
