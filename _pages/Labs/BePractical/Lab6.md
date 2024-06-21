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

## sign-up

first register new account and login nothing new 

<img src="/assets/img/bepractical/lab6/1.png" alt="signup1">


Go back and let's check the reset password function. But this time, there is nothing vulnerable in the reset password function, so what should I do now?
 I decided to check the signup page, so I went to my notes in Notion about this and found this tip

```bash
if the weebsite has no email verification
when sign up try to regeister using existing email by adding \n at the end 
example
{"email":"mostafa.nada1221@gmail.com"}
|
{"email":"mostafa.nada1221@gmail.com\n"}

it could sign you into someone's account/overwrite his account 
```
## Register two accounts

But here, it worked for me with the same email. Let's thread the steps:

* Created the first account with these credentials `email: mostafa123` (as shown in the above photo).
* Created another account with the same email but with a different password `email: 123456`

<img src="/assets/img/bepractical/lab6/3.png" alt="signup2">

When I went back to log in with the second account, it logged me into the first account with the password `mostafa123`.

<img src="/assets/img/bepractical/lab6/2.png" alt="login">


So, Let's go to signup page and register a new account with admin email `admin@bepractical.tech:123456`

<img src="/assets/img/bepractical/lab6/4.png" alt="signup admin">

## Login as admin

Go back to the login, then enter these credentials. It will log you into the admin account.

<img src="/assets/img/bepractical/lab6/5.png" alt="login admin">

Admin account .... Hacked gg


