---
layout:	post
title:	"LaCasaDePapel HTB Walkthrough"
date:	2019-06-27	
categories:	[HTB, walkthrough]
author:	4n4m1k
---

<H4>Hello!</H4>
<H4>LaCasaDePapel just retired today and this my writeup for it. This is my first writeup so if you have any feedback then please let me know.<H4>
<H4>This was an interesting box with some CTF type features. Overall, I had fun hacking this box.</H4>
<br/>

<H1>Enumeration:  </H1>
<H4>Nmap</H4>
First thing that I start with is, Port Scanning.  
<br/>
`nmap -sC -sV -oA nmap/initial 10.10.10.131 -vvv`
<br/>
{% highlight HTML %}
PORT    STATE SERVICE  REASON  VERSION
21/tcp  open  ftp      syn-ack vsftpd 2.3.4
22/tcp  open  ssh      syn-ack OpenSSH 7.9 (protocol 2.0)
| ssh-hostkey: 
|   2048 03:e1:c2:c9:79:1c:a6:6b:51:34:8d:7a:c3:c7:c8:50 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDNzmarvyIINA+hjsLo2xYn1PUyzuTflhtXQs8S1Z56FzbdzXs6FiwhoRGn63XuGCHqCfEzHmh1cg4HGLfGAMwe+AsdJ8hLd/ISNRECH8yvM+9k78Aio3pe+lYbiWSQWyJrQdeqJXyDJFSd6BR3Cr6/rwSvE7N3eWeQvxS+fsg5HOER6n8SOnXvqpWYUo+XmZxGzmluNfsqoJ6doJCyW3X4sTImTlpmRmee6iseo9neZO18aHsARxlkHCqUhp1SBzIiik3DurtH1tgrn8ntfNiK3q0FZJmh9qzu0P/L50j8bzlJdvAsLuqbYmVZhqFs0JfBVdyVTFMn4O0J+IqRrXAF
|   256 41:e4:95:a3:39:0b:25:f9:da:de:be:6a:dc:59:48:6d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNli8Xx10a0s+zrkT1eVfM1kRaAQaK+a/mxYxhPxpK0094QFQBcVrvrXb3+j4M8l2G/C9CtQRWVXpX8ajWhYRik=
|   256 30:0b:c6:66:2b:8f:5e:4f:26:28:75:0e:f5:b1:71:e4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB2uNaKo2PK5cMci4E7dNWQ6ipiEzG3cWUR56qqMZqYR
80/tcp  open  http     syn-ack Node.js (Express middleware)
|_http-favicon: Unknown favicon MD5: 621D76BDE56526A10B529BF2BC0776CA
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: La Casa De Papel
443/tcp open  ssl/http syn-ack Node.js Express framework
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Server returned status 401 but no WWW-Authenticate header.
|_http-favicon: Unknown favicon MD5: 621D76BDE56526A10B529BF2BC0776CA
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-title: La Casa De Papel
| ssl-cert: Subject: commonName=lacasadepapel.htb/organizationName=La Casa De Papel
| Issuer: commonName=lacasadepapel.htb/organizationName=La Casa De Papel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2019-01-27T08:35:30
| Not valid after:  2029-01-24T08:35:30
| MD5:   6ea4 933a a347 ce50 8c40 5f9b 1ea8 8e9a
| SHA-1: 8c47 7f3e 53d8 e76b 4cdf ecca adb6 0551 b1b6 38d4
| -----BEGIN CERTIFICATE-----
| MIIC6jCCAdICCQDISiE8M6B29jANBgkqhkiG9w0BAQsFADA3MRowGAYDVQQDDBFs
| YWNhc2FkZXBhcGVsLmh0YjEZMBcGA1UECgwQTGEgQ2FzYSBEZSBQYXBlbDAeFw0x
| OTAxMjcwODM1MzBaFw0yOTAxMjQwODM1MzBaMDcxGjAYBgNVBAMMEWxhY2FzYWRl
| cGFwZWwuaHRiMRkwFwYDVQQKDBBMYSBDYXNhIERlIFBhcGVsMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEAz3M6VN7OD5sHW+zCbIv/5vJpuaxJF3A5q2rV
| QJNqU1sFsbnaPxRbFgAtc8hVeMNii2nCFO8PGGs9P9pvoy8e8DR9ksBQYyXqOZZ8
| /rsdxwfjYVgv+a3UbJNO4e9Sd3b8GL+4XIzzSi3EZbl7dlsOhl4+KB4cM4hNhE5B
| 4K8UKe4wfKS/ekgyCRTRENVqqd3izZzz232yyzFvDGEOFJVzmhlHVypqsfS9rKUV
| ESPHczaEQld3kupVrt/mBqwuKe99sluQzORqO1xMqbNgb55ZD66vQBSkN2PwBeiR
| PBRNXfnWla3Gkabukpu9xR9o+l7ut13PXdQ/fPflLDwnu5wMZwIDAQABMA0GCSqG
| SIb3DQEBCwUAA4IBAQCuo8yzORz4pby9tF1CK/4cZKDYcGT/wpa1v6lmD5CPuS+C
| hXXBjK0gPRAPhpF95DO7ilyJbfIc2xIRh1cgX6L0ui/SyxaKHgmEE8ewQea/eKu6
| vmgh3JkChYqvVwk7HRWaSaFzOiWMKUU8mB/7L95+mNU7DVVUYB9vaPSqxqfX6ywx
| BoJEm7yf7QlJTH3FSzfew1pgMyPxx0cAb5ctjQTLbUj1rcE9PgcSki/j9WyJltkI
| EqSngyuJEu3qYGoM0O5gtX13jszgJP+dA3vZ1wqFjKlWs2l89pb/hwRR2raqDwli
| MgnURkjwvR1kalXCvx9cST6nCkxF2TxlmRpyNXy4
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
| tls-nextprotoneg: 
|   http/1.1
|_  http/1.0
Service Info: OS: Unix
{% endhighlight %}

<br/>

<br/>

Page on `http://10.10.10.131`
<br/>
<img src="/images/hackthebox/lacasa/80.png" width="90%">
<br/>
<br/>
Page on `https://10.10.10.131`
<br/>
<img src="/images/hackthebox/lacasa/website.png" width="90%">
<br/>

<br/>
<H1>Exploitation:</H1>
I started to search exploits for the services found on nmap. 
By searching exploit of vsftpd 2.3.4 on google, we land to this page 
`https://sweshsec.wordpress.com/2015/07/31/vsftpd-vulnerability-exploitation-with-manual-approach/`
<br/>
The exploit: 
If we try to log into the FTP server with a username ending with `:)` and any password, then a backdoor opens at port `6200`.
<br/>
{% highlight HTML %}
ftp 10.10.10.131
Connected to 10.10.10.131.
220 (vsFTPd 2.3.4)
Name (10.10.10.131:an4m1k): user:)
331 Please specify the password.
Password:


{% endhighlight %}
The shell freezes after this.

Let's try to connect to port `6200` as per the exploit.
<br/>
`nc -nv 10.10.10.131 6200` 
<br/>
![](/images/hackthebox/lacasa/6200.png)
<br/>
It looks like a php shell, so I assumed that it will interpret php commands. If we try any system commands then the system doesn't recognize those commands.
After trying some commands, I found something interesting.
<br/>
![](/images/hackthebox/lacasa/6200 help.png)
<br/>
After trying multiple commands with `sudo`, I found some commands that worked.
<br/>
`scandir:` Returns an array of files and directories from directory.
<br/>
![](/images/hackthebox/lacasa/scandir.png)
<br/>
<br/>
We can see the files and we can read the files by using `file_get_contents`.
<br/>
<br/>
If we looked at the page on `https://10.10.10.131`, you can see that we get certificate error.
<br/>
I found `ca.key` in the directory `/home/nairobi` <br/>
![](/images/hackthebox/lacasa/cakey.png)
<br/>
If you wondering whether I can read files then I can read `user.txt`. But that is not the case. I don't have permission to read `user.txt`.
<br/><br/>
We can see that we need a certificate to access the site on `https://19.10.10.131/` and we have a `ca.key` from the shell on `port 6200`<br/>
The next step is to create a client certificate using `ca.key` and the existing certificate that we export from `https://10.10.10.131/`
<br/><br/>
The resources that I used for this section :<br/>
1. <https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs><br/>
2. <https://medium.com/@sevcsik/authentication-using-https-client-certificates-3c9d270e8326><br/>
<br/>



To create client certificate, we have to generate a Private Key and Certificate Signing Requests(CSR)<br/>
`openssl req  -newkey rsa:2048 -nodes -keyout mykey.key -out mykey.csr`
<br/>


The next step is to create a client certificate by using `ca.key`<br/>
`openssl x509 -req -in mykey.csr -CA lacasadepapel_htb.crt -CAkey ca.key -out myclient.pem -set_serial 01 -days 365`
<br/>


To import this certificate to Firefox, we have to convert it into PKCS12 format.<br/>
`openssl  pkcs12 -export -inkey domain.key -in client.pem -out client.p12`
<br/>
<br/>
I imported `client.pk12` in Firefox and reloaded the web page.
<br/>
<img src="/images/hackthebox/lacasa/privatearea.png" width="90%">
<br/>


We can see a nice private area. After clicking around, I found something interesting. The url `https://10.10.10.131/?path=SEASON-1` uses `path` to search for files.
If we enter a valid path following the `path=`, we can read the contents of the directories.
<br/>
<img src="/images/hackthebox/lacasa/ssh1.png" width="90%">
<br/><br/>
Now we can't just download the file or read the contents of the file.<br/>
I used Burpsuite to see what requests are sent while downloading a file. You can do that by starting Burp and clicking on any file in Season-1 folder.<br/>
<img src="/images/hackthebox/lacasa/burp.png">
<br>


The request sent is `GET /file/U0VBU09OLTEvMDEuYXZp`, where the file name is encoded in base-64. You can verify it by `echo 'U0VBU09OLTEvMDEuYXZp' | base64 -d`<br/><br/>
Now I want something to that will help me log into the machine. There is `.ssh` folder, I can try to download the private key `id_rsa` and see if I hit any luck.
<br/><br/>
To download that we have to convert the path into base64 - `echo -n ../.ssh/id_rsa | base64`. We get `Li4vLnNzaC9pZF9yc2E=`<br/>
<br/>
It worked!<br/:w
>
<img src="/images/hackthebox/lacasa/idrsa.png" width="90%">
<br/><br/>
Now we can just have to login using the private key. For user, I had to do trial and error to find out that that the key was for the user `professor` <br/>

<img src="/images/hackthebox/lacasa/ssh.png"><br/>


Once I am logged in, I can't read `user.txt` `:(`<br/><br/>
But I see some interesting files in the `professor's` home directory.<br/>

<img src="/images/hackthebox/lacasa/priv.png"><br/>


By looking at the processes by using `ps` command, we can see that `/usr/bin/node` is executing whatever is in `memcached.js` and `memcached.ini` is file that calls that process. 

<img src="/images/hackthebox/lacasa/priv1.png"><br/>


If we can edit or change this `memcached.ini` file, then we can run anything as root. After some trial and error, I found out that, I cannot edit `memcached.ini` but I can move it and place my own `memcached.ini` file.

<br/>
<img src="/images/hackthebox/lacasa/priv2.png">
<br/>
<br/>
We have root!<br/>
<br/>
<img src="/images/hackthebox/lacasa/root.png">
<br/><br/>
I can read both `user.txt` and `root.txt` flags.<br/>
<img src="/images/hackthebox/lacasa/flags.png">
