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
first register new account and login nothing new. So, let's check reset password function 

## Reset Password 

type the email and click send 

<img src="/assets/img/bepractical/lab4/1.png" alt="reset password">

ok now check the email to see the otp 

<img src="/assets/img/bepractical/lab4/2.png" alt="reset link">

But this time, there is a reset link instead of an OTP like in previous labs. So, let's copy the link to check it

## Crack The Hash

<img src="/assets/img/bepractical/lab4/3.png" alt="link">

I noticed the term `Secret_key` followed by a value. I think this is some kind of hash. At first, I considered cracking this hash on the CrackStation website, but I didn't get the value. Since it's `32-bit`, I thought it might be `MD5`. To be sure, I took this value to a hash analyzer, and yes, it's `MD5`.

I think this hash is something related to the user. So, we can try various strings like name, username, email address, phone number, password, etc.

By trying the email address used to log in, I got a match

<img src="/assets/img/bepractical/lab4/4.png" alt="hash match">

We now go back to the `Forgot Password` functionality, type in the admin account email, and click `send`

<img src="/assets/img/bepractical/lab4/5.png" alt="reset admin">

"So now, let's get the hash for the admin account email address we are trying to take over."

<img src="/assets/img/bepractical/lab4/6.png" alt="admin hash">

let's take the admin hash as `secret_key` then copy the link and paste it in firefox 

<img src="/assets/img/bepractical/lab4/7.png" alt="admingg">

Admin account .... Hacked gg



