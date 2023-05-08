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
