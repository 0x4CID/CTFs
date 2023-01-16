# CTF Challenge - VulnLawyers

## Intro

This is my writeup for the CTF challenge VulnLawyers. The domain we are given at the beginning is http://www.vulnlawyers.co.uk/

## Flag 1


## Flag 2

The second flag (the first one I found) was exposed by a broken redirect. When visiting `www.vulnlawyers.co.uk/login` we were redirected to `http://www.vulnlawyers.co.uk/denied` however before we were redirected we were able to view the response (and the flag) of the login page.

[^FLAG^FB52470E40F47559EBA87252B2D4CF67^FLAG^]

Another bit of info that was exposed was a path to `/lawyers-only`