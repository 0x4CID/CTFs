# CTF Challenge - VulnBegin

## Intro

This is my writeup for the CTF challenge VulnBegin. The domain we are giving at the beginning is http://www.vulnbegin.co.uk

## Flag 1

This flag can be found be found as a TXT record when enumerating DNS records.


```
┌──(kali㉿kali)-[~]
└─$ dnsrecon -d vulnbegin.co.uk      
[*] std: Performing General Enumeration against: vulnbegin.co.uk...
[-] DNSSEC is not configured for vulnbegin.co.uk
[*]      SOA ns1.digitalocean.com 173.245.58.51
[*]      SOA ns1.digitalocean.com 2400:cb00:2049:1::adf5:3a33
[*]      NS ns1.digitalocean.com 173.245.58.51
[*]      Bind Version for 173.245.58.51 "Salt-master"
[*]      NS ns1.digitalocean.com 2400:cb00:2049:1::adf5:3a33
[*]      NS ns2.digitalocean.com 173.245.59.41
[*]      Bind Version for 173.245.59.41 "Salt-master"
[*]      NS ns2.digitalocean.com 2400:cb00:2049:1::adf5:3b29
[*]      NS ns3.digitalocean.com 198.41.222.173
[*]      Bind Version for 198.41.222.173 "Salt-master"
[*]      NS ns3.digitalocean.com 2400:cb00:2049:1::c629:dead
[*]      A vulnbegin.co.uk 68.183.255.206
[*]      TXT vulnbegin.co.uk [^FLAG^BED649C4DB2DF265BD29419C13D82117^FLAG^]
[*] Enumerating SRV Records
[+] 0 Records Found


```


## Flag 2

When carrying on general recon I decided to look at crt.sh which is a general repository on domain certificates. I found the subdomain `v64hss83.vulnbegin.co.uk` which when visiting presents you with the second flag.

[^FLAG^047524FE61AE6B5FD1D184994C7322FC^FLAG^]


## Flag 3