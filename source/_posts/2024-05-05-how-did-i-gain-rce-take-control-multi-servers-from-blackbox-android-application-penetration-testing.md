---
title: 'How did I gain RCE and dump the database from  Android Application'
date: 2024-05-05 23:48:05
tags:
- Login
- Bypass
- MSSQL
- Metasploit
- injection
category:
- SecurityResearch
index_img: /assets/images/img/MSSQL/mssql-logo.png
banner_img: /assets/images/img/MSSQL/mssql-logo.png
author: 'Salah'
description: 'Chain of attackes lead to remote code execution MSSQL servers'
subtitle: 'MSSQL Servers RCE.'
---

**Description**

Hi Hackers My name is Mahmoud Salah AKA H3X0S3, Offensive Security Engineer. In this article, I will explain how I got full DB access and then got RCE on the DB server. let’s start. It was Android application penetration test engagement.

---

At first, I tried to decompile the application and it wan not obfuscated and there noting was interesting, so list start to run the application of android device.

After running the application on device, i tried to intercept the requests using burpsuite.

![image](/assets/images/img/MSSQL/ssl.png)

➜ I tried to brute force end points and found endpoint vulnerable to directory lisiting.

![image](/assets/images/img/MSSQL/directory listing.png)

➜ I tried to open each directory in this endpoint and access any files but i get 404 - File or directory not found.

Hmmmmmm, i find a Publish.rar and try to download it, WOW successfully download.

let's discover the files in comprised file.  
it contains  
1- dll files  
2- Blank HTML pages  
3- Packages  
4- Web.config HEEE  
I find multi IP MSSQL servers with credentials and smtp server.

![image](/assets/images/img/MSSQL/blure config.png)

➜ I tried to test if this credentials it valid or not using metasploit.

![image](/assets/images/img/MSSQL/metaploit.jpg)

➜ Enable xp_cmdshell

```bash  
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1;  
GO

-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO

-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1;  
GO

-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO

-- To set "show advanced options" back to false  
EXECUTE sp_configure 'show advanced options', 0;  
GO

-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
````

  
![image](/assets/images/img/MSSQL/enable xp_cmdshell.jpg)

➜ Try to execute command

![image](/assets/images/img/MSSQL/run command.jpg)

Finally we can execute and run commands

![image](/assets/images/img/MSSQL/wow.gif)

