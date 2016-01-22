---
title: How to install a PositiveSSL certificate provided by Namecheap
date: 2014-01-22 04:12:21 
tags: code
layout: post
---
I recently transferred all of my domains from 123-reg to Namecheap. Namecheap have recently given their website a makeover; it wasn't anything to look at before but it's lovely now! 

With the transfers I had the opportunity to buy SSL certificates for £1.20 (for the year). Who the hell would pass on that? I have no distinct need for HTTPS on this site. I don't store or process any secure details, there's no tokens being passed back and forth to an API - but it's always better to use HTTPS than not. 

Anyway, I thought I'd share how to install a PositiveSSL (from Comodo) certificate provided by Namecheap and enable it for use with Nginx. 

My VPS is provided by Linode, and runs Debian. But the instructions should work for most Linux distros. 

* Buy an SSL certificate. Full price the PositiveSSL certificate is only £5.48 for the year. 

* First we'll generate a private key `openssl genrsa -des3 -out <yourdomain.com>.key 2048`

* Next we'll create a nopass version, this is so Nginx can use it without having to enter the password you chose. `openssl rsa -in <yourdomain.com>.key -out <yourdomain.com.key.nopass>`

* Next generate the Certificate Signing Request with `openssl req -new -key <yourdomain.com>.key.nopass -out <yourdomain.com>.csr`

* Answer all the questions you're prompted with. The most important thing here is that you enter your domain name for 'Common Name'. If you enter your domain without the WWW, with the PositiveSSL certificate both WWW and non-WWW will be covered. But this varies between certificates I believe.

* Open up the .csr file that's just been generated. Copy the contents, including the `-----BEGIN CERTIFICATE REQUEST-----` and `-----END CERTIFICATE REQUEST-----` markers. 

* If you're using Namecheap you'll now want to go to your SSL control panel, by clicking 'Manage SSL Certificates'. Click 'Activate' next to the certificate you want to use.

* Paste in the contents of the .csr file that we just copied, and answer any questions on the page. You'll be asked to provide emails to A) Prove domain ownership and authorisation of the request and B) to email the certificates to. 

* When the first e-mail arrives it'll contain a verification code, copy this code and enter it on the page that the link within the e-mail takes you to.

* Once the code has been entered, and everything is verified, your actual certificates will be e-mailed out. 

* When the e-mail arrives, download all 3 files attached to the e-mail. They should just arrive as a .zip file.

* Upload / copy these files to your server. I store mine in a sensible location that will hold all SSL certificates. 

* `cd` in to this directory, and run `cat <yourdomain_com>.crt PositiveSSLCA2.crt AddTrustExternalCARoot.crt > <yourdomain.com>.pem`

* Phew. All done on the certificate side. Now we just need to configure Nginx by slightly changing the configuration file.

* Within your relevant server block you'll want to have this:

<pre>
	<code>
server {
    listen  443 ssl;
	ssl_certificate	/some/location/ssl/yourdomain.com.pem;
	ssl_certificate_key	/some/location/ssl/yourdomain.com.key.nopass;
}
    </code>
</pre>

* Last but not least chances are you'll want HTTPS used exclusively, so we'll re-direct regular port 80 HTTP traffic. Ignore this if you're going to allow both HTTP and HTTPS to be used. Enter the following within your Nginx configuration: 

<pre>
	<code>
server {
	listen    80;
    server_name    yourdomain.com;
    rewrite ^(.*) https://www.yourdomain.com$1 permanent;
}
    </code>
</pre>

* Restart Nginx, and start browsing your site using HTTPS :)

That's the whole process. This was my first time enabling SSL. It's certainly not hard, but it is quite a laborious process, It's definitely worth it though, even if you don't need it need it. For those sites who do need SSL enabled, and don't though, shame on you - SSL certificates are too cheap for excuses nowadays.  



