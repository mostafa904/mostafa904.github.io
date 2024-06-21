---
title: "Account TakeOver Lab4"
tags:
    - Lab
    - Bepractical
date: "2024-06-20"
thumbnail: "/assets/img/thumbnail/labs.png"
bookmark: true
---
## Description

As usual, try logging in with `admin@bepractical.tech`.
first register new account and login nothing new go back let's check reset password function 

## Reset Password 

type the email and click send 

<img src="/assets/img/bepractical/lab4/1.png" alt="reset password">

ok now check the email to see the otp 

<img src="/assets/img/bepractical/lab4/2.png" alt="reset link">

but this time there is a reset link not otp like previous labs so lets copy the link to check it 

## Crack The Hash

<img src="/assets/img/bepractical/lab4/3.png" alt="link">

i noticed the term `Secret_key` followed by a value i think this is some kind of hash 
first i think to crack this hash in crack station website but i didnt got the value 
->32bit so i think its `MD5` to be sure i took this value to hash analyzer and yes its md5 

i think that this hash is something relative to the user So we can try various strings like name, username, email address, phone number, password…etc.

Trying the email address used to login, I got a match

<img src="/assets/img/bepractical/lab4/4.png" alt="hash match">

We now go back to the `Forgot Password` functionality and type in the admin account email and click `send`

<img src="/assets/img/bepractical/lab4/5.png" alt="reset admin">

So now lets us get the hash for the admin account email address we are trying to takeover.

<img src="/assets/img/bepractical/lab4/6.png" alt="admin hash">

let's take the admin hash as `secret_key` and copy the link then paste it in firefox 

<img src="/assets/img/bepractical/lab4/7.png" alt="admingg">

Admin account .... Hacked gg



