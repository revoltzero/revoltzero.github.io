---
title: "TempImage -Hacker101"
date: 2018-07-08T12:22:40+06:00
image: images/blog/6.png_(1).png
author: Shahabor HOssain Rifat
description: "This is meta description"
---



### Hello, it's ShahRiffy. In this write-up, I'll walk you through the process of solving the Templmage challenge in the HackerOne CTF. Let's dive right in!

### In this challenge, they present a web application, and they provide with a trial version of this application.

![2.png](/exampleSite/static/images/blog/2.png)

### At the outset, I attempted to upload a file, only to encounter an error. It was then that I realized the supported format was limited to `PNG` image files.

![3.png](/exampleSite/static/images/blog/3.png)

### Upon trying to upload a PNG file, I observed that the image successfully made its way to the `/files` directory on the web server. Notably, it carried a uniquely generated prefix before the actual file name.

### It looked something like this

![4.png](TempImage%20-Hacker101%2049e62c7a60254765915c97398fc91f7d/4.png)

### When I discover paths, the first vector that immediately springs to my mind is `Path Traversal`

### Exploring the system for a potential path traversal vulnerability, our goal is to gain the ability to upload an image to a directory outside of the default `/files` directory.

### By utilizing Burpsuite to inspect the file upload POST request, we can manipulate the upload path for our image.

![5.png.png](TempImage%20-Hacker101%2049e62c7a60254765915c97398fc91f7d/5.png.png)

### **Inserting `/../../` before the file name allows us to navigate two steps back from the current location. For example, it changes the path from `/index/files/image` to `/index`, where we can attempt to upload the image.”**

![6.png (1).png](/images/blog/6.png_(1).png)

### yeah! we got the `Flag0` , the server is indeed vulnerable to path traversal attacks.

### Knowing that we have the capability to upload a file to a path that can execute PHP code opens up numerous possibilities for malicious activities

### Among the methods, one involves the insertion of a webshell backdoor code into the content of an image. This technique is widely recognized for exploiting situations where the server does not inspect the content of uploaded image files.

### Let's dive into the attack

### Continuing with `test.png`, as we proceed to inspect our POST request in Burp, we can observe that the POST body contains an extensive sequence of symbols, which represents the binary content of the image.

### We have the opportunity to inject our malicious payload at the tail end of the image's content, allowing it to be executed by the server.

![Untitled](TempImage%20-Hacker101%2049e62c7a60254765915c97398fc91f7d/Untitled.png)

### I've inserted a PHP code containing a malicious payload at the end of the image's content, and altered the file extension to `.php`.

 

![Untitled](TempImage%20-Hacker101%2049e62c7a60254765915c97398fc91f7d/Untitled%201.png)

### Note : Clicking on “Follow redirect” giving us 200 OK with the content of our new *`test.php`* file, that includes its content, and more importantly — the response to the command that we injected and got executed.

### We have yet to retrieve our flag.

### After some exploration, I discovered that the flag is situated within the `index.php` page.

![Untitled](TempImage%20-Hacker101%2049e62c7a60254765915c97398fc91f7d/Untitled%202.png)

### **References**

• Diagrams were drawn using [Excalidraw](https://excalidraw.com/)