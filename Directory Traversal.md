# Directory Traversal

## Lab 1 : File path traversal, simple case

This lab contains a file path traversal vulnerability in the display of product images.

To solve the lab, retrieve the contents of the /etc/passwd file.

### Sol :

Open any image from the listed products.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/a11f4a23-7604-4bc7-8c55-465e8fc5b8e5)

We can see filename in the URL of the image.

Traverse back from that directory by using `../` 3 times so that to get to the `root` directory

Now traversing to `/etc/passwd` to get the passwd file.

The command will be : `../../../etc/passwd`

      image?filename=../../../etc/passwd
      
`passwd` file is accessed.

Thus, the lab is solved.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/ba64c863-0067-4198-a363-2c5412b9b0f9)

## Lab 2 : File path traversal, traversal sequences blocked with absolute path bypass

This lab contains a file path traversal vulnerability in the display of product images.

The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.

To solve the lab, retrieve the contents of the /etc/passwd file.

### Sol :

Using Burp Suite to intercept and modify a request that fetches a product image

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/1af17322-d279-4282-b375-648d0c166e09)

Replace the product image value with `/etc/passwd`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/05e2a5ec-ea14-4e0e-970c-5fc8452b3303)

We got the `passwd` file contents and lab is solved.

## Lab 3 : File path traversal, traversal sequences stripped non-recursively

This lab contains a file path traversal vulnerability in the display of product images.

The application strips path traversal sequences from the user-supplied filename before using it.

To solve the lab, retrieve the contents of the /etc/passwd file.

### Sol :

Using Burp Suite to intercept and modify a request that fetches a product image

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/44bc4ec7-b32d-47b3-9fdb-450078b7218d)

Replace the product image value with `....//....//....//etc/passwd`

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/6686f90b-43d8-4e85-a98d-4991d65c394e)

We will get the passwd file and lab is solved.

![image](https://github.com/tousif13/Port_Swigger_Labs/assets/33444140/d86721cd-bbd9-4007-8ef5-5d129f9fb3f1)
