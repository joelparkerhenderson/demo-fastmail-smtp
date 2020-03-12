# Demo Fastmail SMTP

Demonstration of:

* <a href="https://fastmail.com">Fastmail.com</a> SMTP

* Using the command line to send email.

Contents:

* [](#)


## Get our provider's SMTP settings

For our example we use Fastmail.com for SMTP for sending email.

https://www.fastmail.com/help/technical/servernamesandports.html

* Server: smtp.fastmail.com

* Port: 465

* SSL/TLS Encryption: Enabled, but not STARTTLS

* Authentication: PLAIN

* Username: Your Fastmail email address, including the domain.

* Password: Your app-specific password. You cannot use your regular Fastmail password.


## Security and TLS

For security, we prefer port 465 with implicit TLS encryption, rather than port 587 with STARTTLS.

https://www.fastmail.com/help/technical/ssltlsstarttls.html


## Create an application password

To connect to Fastmail via SMTP, we create an application password.

https://www.fastmail.com/settings/security/devicekeys


## Encode SMTP username and SMTP password

We need to base64 encode the SMTP username and SMTP password.

Example using the command `base64`:

```sh
$ echo -n "alice@example.com" | base64
YWxpY2VAZXhhbXBsZS5jb20=

$ echo -n "mysecret" | base64
bXlzZWNyZXQ=
```

Example using the command `openssl`:

```sh
$ echo -n "alice@example.com" | openssl enc -base64
YWxpY2VAZXhhbXBsZS5jb20=

$ echo -n "mysecret" | openssl enc -base64
bXlzZWNyZXQ=
```


## Connect

Connect via the command line:

```sh
% openssl s_client -connect smtp.fastmail.com:465
CONNECTED(00000006)
…
220 smtp.fastmail.com ESMTP ready
```

Login:

```smtp
AUTH LOGIN
```

Response:

```smtp
334 VXNlcm5hbWU6
```

Input the SMTP username base64:

```smtp
YWxpY2VAZXhhbXBsZS5jb20=
```

Response:

```smtp
334 UGFzc3dvcmQ6
```

Input the SMTP password base64:

```smtp
bXlzZWNyZXQ=
```



## Verify MX

For our example we use the domain joelparkerhenderson.com.

Verify that the internet knows about our domain MX records.

Example:

```sh
% nslookup -type=mx joelparkerhenderson.com
Server:		103.86.99.99
Address:	103.86.99.99#53

Non-authoritative answer:
joelparkerhenderson.com	mail exchanger = 20 in2-smtp.messagingengine.com.
joelparkerhenderson.com	mail exchanger = 10 in1-smtp.messagingengine.com.
…
```


## Get our local IP

To test on our local system, we need to know our local IP system's public address.

One way to get the IP address is http://whatismyipaddress.com/

Result: 45.152.4.52

Look up the DNS PTR record:

```sh
nslookup -type=ptr 45.152.4.52
```
