# OS Command Injection
> OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data. Very often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, exploiting trust relationships to pivot the attack to other systems within the organization.

## Lab 1 : OS command injection, simple case

This lab contains an OS command injection vulnerability in the product stock checker.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the whoami command to determine the name of the current user.

### Sol :

If we click a product and click check stock. then, we will get the ProductId & StoreId

![image](https://user-images.githubusercontent.com/33444140/236799000-1d2eeac3-d715-48e9-ae44-6bfa91b84c9f.png)

Then, we are giving 
