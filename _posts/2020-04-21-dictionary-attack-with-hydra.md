---
title: Guessing Login passwords with Hydra
tags: [Ethical Hacking, Web Penetration testing]
style:
color:
description: Launching dictionary attacks on login forms using Hydra.
---
<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

This post will help us learn how to create our own dictionary of passwords and then launch atatcks on login forms using the dictionary we created along with the **Kali based tool Hydra**. These days almost all the websites contain authentication forms. Therefore, this tool is very handy when launching bruteforce attacks. So, before getting our hands dirty with the tool; let us first polish some of our basics about dictionary and bruteforce attacks.

# Bruteforce attack vs Dictionary Attack
We often use bruteforce and dictionary attack interchangeably and it is somewhat right but there is a subtle difference between them. We can think bruteforce attack as a **superset** of dictionary attack. Bruteforce attacks are exhaustive attacks which tries all the possibilities (viz. letters, characters, special characters, and their combinartions as well) where as in dictionary attack we provide a dictionary i.e. list of words(can be letters, characters and also their combinations) from which the target system is penetrated with. **Brute force** attacks being exhaustive will ensure that you'll get the field data(password filed, username, etc.), but as it is exhaustive; the time taken will vary(will be enormous for secured passwords). On the other hand, **dictionary attcks** are selective so, it may provide false positives but it is fast. So, practically we prefer dictionary attacks rather than bruteforce.

# Creating our own dictionary with crunch
So before we begin launching attack with hydra, we must be ready with our dictionary of passwords which we are going to use. Although we can use the dictionaries which are already present on the internet([**my favorite repo**](https://github.com/danielmiessler/SecLists)); but in case you want to be more specific with the target you might require to create your own. There is a very handy tool called [**_crunch_**](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15&cad=rja&uact=8&ved=2ahUKEwigneWshvnoAhUlzTgGHREsBv4QFjAOegQIDRAR&url=https%3A%2F%2Fwww.hackingtutorials.org%2Fgeneral-tutorials%2Fcrunch-password-list-generation%2F&usg=AOvVaw0BMw909BGMczMnzcP93BFO) which helps in doing this task for us.

You can generate the dictionary that fits your needs. So, Let us generate a dictionary which contains password with length 8 and has letters abcd and numbers 1234. 
```bash
~# crunch 8 8 abcd1234 -o /Desktop/dict.txt -t a@@@@@@4
Crunch will now generate the following amount of data: 2359296 bytes
2 MB
0 GB
0 TB
0 PB
Crunch will now generate the following number of lines: 262144 

crunch: 100% completed generating output
```
###### Command overview:

* **_crunch_** - name of the tool
* **_8_** - minimum length
* **_8_** - maximum length(Since I want to create smaller wordlist so I took both the lengths same)
* **_-o_** - Location where the dictionary gets created
* **_-t_** - Pattern if any of the passwords(In the above case all the passwords in the dictionary should start with a and end in 4)

There are however numerous options of creating wordlists using crunch. You can explore all the options to get the better insight. I presume you already know **how to man any tool in linux** :thinking:[man crunch].

# Guessing Login password Using a wordlist attack with Hydra:
Now, that our dictionary is ready next thing to do is to launch the wordlist attack. The tool which we are going to use is Hydra(as evident in the blog title obviously:sweat_smile:). Hydra is a very simple tool to use but in the contrary very powerful and efficient in launching bruteforce and dictionary attacks on almost any authentication services(like routers, web applications, etc.) which I can think of.

Using the tool is simple. Fire up the terminal and type the command below:

**SYNTAX:** hydra [IP] -L[usernames] -P[password list] [service]
###### Command overview:
* **_hydra_** - Name of the tool
* **_IP_** - IP address of the target website (I presume you can **convert the domain names to ip addresses**:thinking:[dig +short www.example.com]) 
* **-L** - List of usernames to bruteforce the target. Use -l for giving just one user.
* **-P** - List of passwords to bruteforce the target. Use -p for providing one password.
* **service** - Providing the type of service you are attacking. This is the most tricky part of the command but is very simple once you get the understanding.

Let us see it in action using an example. I am using [**metasploitable**](https://information.rapid7.com/download-metasploitable-2017.html) machine to demonstrate this example. The application name is **dvwa**.

* We have to launch the attack on dvwa web application as shown below:

{% include elements/figure.html image="https://mandy8055.github.io/assets/2020-04-21-dvwa-page.png" caption="Page to exploit" %}

* From the page we have got our **IP address**, the **protocol on which it is running(http)** and **Failure login Message**(_Don't worry!I'll let you know where the fromer two parts will be useful_).

* Now the next thing, which we need to do is to inspect the **action type** of the form which this web application is using(I presume you'll be knowing **how to know the form action type:thinking**:[Write click on the form and click on **_inspect element_**]).By doing this; we came to know about the form type i.e. **POST**. If you'll know the [difference between GET and POST](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=12&cad=rja&uact=8&ved=2ahUKEwj8xJn8tfnoAhVUfisKHQZ1CoQQFjALegQIAhAB&url=https%3A%2F%2Fwww.w3schools.com%2Ftags%2Fref_httpmethods.asp&usg=AOvVaw1Jv4zkrWlgxGHp_1W9ypYB), you'll know that POST requests are hidden. They are not visible in the url.

* Next thing which we need is to do is to find out what are the **parameters** which are sent to the server along with username and password. To know this, we are going to use [**BurpSuite**](https://portswigger.net/burp); another very helpful tool for hackers. I'll talk about it in some other blog later. For quick gist, I'm using **BurpSuite's proxy to intercept HTTP requests**. Below is the image which shows the parameters that are sent to dvwa server when I used _username as admin and password as somerandompass_.

{% include elements/figure.html image="https://mandy8055.github.io/assets/2020-04-21-burp-screen.png" caption="Intercepted POST request" %}

* That's it; we have all the things needed to construct our command for hydra. Now, let us write the command and understand its various parts in detail.

**Command:** hydra 192.168.43.205 -l admin -P dict.txt http-form-post "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed"

###### Command Overview[The service Part]:

* **_http-form-post_** - It is the name of the service which tells hydra what the web app is using. You can man it to get more info.
* **_"/dvwa/login.php:username=^USER^&password=^PASS^&login=Login:F=Login Failed"_** - This part which is enclosed in quotes contain three colons. Let us break them down one by one.
    * I<sup>st</sup> part: This part of the colon represents the page where the attack needs to be launched.
    * II<sup>nd</sup> part: This part of the colon represents the parameters that are passed to dvwa server everytime the user tries to log in(**The reason why we used Burp proxy**).
    * III<sup>rd</sup> part: This part of the colon shows what is the text that will be visible on the page when login failure(because we used **F** option) occurs.

Finally, fire up the terminal and paste this command, and you'll see the result below:

```bash
~# hydra 192.168.43.205 -l admin -P dict.txt http-form-post "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-04-21 16:14:47
[DATA] max 16 tasks per 1 server, overall 16 tasks, 262146 login tries (l:1/p:262146), ~16385 tries per task
[DATA] attacking http-post-form://192.168.43.205:80/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed
[STATUS] 504.00 tries/min, 504 tries in 00:01h, 261642 to do in 08:40h, 16 active
[80][http-post-form] host: 192.168.43.205   login: admin   password: a11bdd44
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-04-21 16:16:02
```

Bingo:nerd_face:, we found the password for the username **admin** i.e. **a11bdd44**. 

Give these tools a try and explore various other features and options available for these tools. Keep hacking for good and keep growing always.




