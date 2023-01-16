# CTF Challenge - VulnBegin

## Intro

This is my writeup for the CTF challenge VulnBegin. The domain we are given at the beginning is http://www.vulnbegin.co.uk

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


## Flag 4

The fourth flag was found in a directory called `/secret_d1rect0y/` which was found in robots.txt

[^FLAG^2B22E2CB70E218510802B0359488F6A2^FLAG^]

## Flag 5

Next running ffuf I found the page `/cpadmin` and a login form at `/cpadmin/login` and was able to brute force the username `admin` (this was discovered due to the specific error message produced when typing the correct username and incorrect password) and password `159753`. After logging in the next flag was found.

[^FLAG^93D7491FB4B054FB5C5AC3E0292BE41C^FLAG^]

Another interesting thing to note was when logging in you are assigned a new cookie `token=2eff535bd75e77b62c70ba1e4dcb2873`. I then tried re-brute forcing the paths I did before and found an additional page that can only be found when brute forcing with the new cookie attached.

## Flag 6

The page I discovered was `/cpadmin/env` where I found the following json `{"api_key":"X-Token: 492E64385D3779BC5F040E2B19D67742","flag":"[^FLAG^F6A691584431F9F2C29A3A2DE85A2210^FLAG^]"}`.