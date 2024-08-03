---
title: "Account TakeOver Lab1"
tags:
    - Lab
    - Bepractical
date: "2024-06-20"
thumbnail: "/assets/img/thumbnail/labs.png"
bookmark: true
---
# Description
---
bepractical.tech. It's a platform that offers various challenges and labs for cybersecurity enthusiasts and bug hunters to practice their skills. The site provides hands-on experience with real-world scenarios, allowing users to test and improve their abilities in finding and exploiting vulnerabilities.
---
## Login

First we will login with the credentials given 
`john@bepractical.tech:john@123`

<img src="/assets/img/bepractical/lab1/1.png" alt="login">

<img src="/assets/img/bepractical/lab1/2.png" alt="login2">

As you can see, we were logged in as John.

Now, let's go back to the login page, intercept the same request with BurpSuite, and send it to the Repeater.

<img src="/assets/img/bepractical/lab1/3.png" alt="burp">

When we logged in with the given credentials, the response was `MQ==`, which is Base64 for `1`.

If we change the email parameter to `admin@bepractical.tech`, the response will be `ZmFsc2U=`, which means `false`.

<img src="/assets/img/bepractical/lab1/4.png" alt="burp2">

## Response Manipulation

Immediately, I thought about `response manipulation`.

So, I went back and intercepted the request. I changed the email to `admin@bepractical.tech`, right-clicked, selected Do intercept, chose response to this request, and then forwarded it. After that, I changed `ZmFsc2U=` to `MQ==` and forwarded the request again 

<img src="/assets/img/bepractical/lab1/5.png" alt="burp3">

Now, turn intercept off, then go back to Firefox.
<img src="/assets/img/bepractical/lab1/admin.png" alt="admin">

GG lab is solved. 
