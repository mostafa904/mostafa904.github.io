---
title: "Account TakeOver Lab2"
tags:
    - Lab
    - Bepractical
date: "2024-06-20"
thumbnail: "/assets/img/thumbnail/labs.png"
bookmark: true
---
## Description

in this lab he want me to takeOver admin account and didnt gave me any credentials to login with

## Sign up

<img src="/assets/img/bepractical/lab2/1.png" alt="sign">


 there is a singup button ,I am using a temporary email address `mohmal.com`
<img src="/assets/img/bepractical/lab2/2.png" alt="temp_mail">

Let's register an account
 <img src="/assets/img/bepractical/lab2/3.png" alt="make account">

 ## Login

After creating an account, let's go back to the login page to enter the account that we created.
 <img src="/assets/img/bepractical/lab2/4.png" alt="access account">

Okay, nice. What should I do now? I tried response manipulation like in the first lab, but it didn't work here.

## Reset Password

So,let's test the reset password function 
 <img src="/assets/img/bepractical/lab2/5.png" alt="reset password">
 
I entered the temporary email address to receive the OTP on it and then clicked send (BurpSuite fired in the background). The OTP has been sent, so I checked my email to get it
  <img src="/assets/img/bepractical/lab2/6.png" alt="OTP">

So, I entered the code and intercepted the request with BurpSuite.
  <img src="/assets/img/bepractical/lab2/7.png" alt="enter OTP">

change the emailforOtp parameter to `admin@bepractical.tech` and forward the request 
  <img src="/assets/img/bepractical/lab2/change.png" alt="change email">

  now go to firefox 
    <img src="/assets/img/bepractical/lab2/8.png" alt="admingg">

Admin account .... Hacked gg
 

