---
layout: post
title: "Let's Encrypt Stuff"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

This post covers setting up SSL certificates using Let's Encrypt on a remote CentOS 7 system running `nginx` as a reverse proxy for multiple micro-services running on the same system.

# Install Certbot
`certbot` is the application that validates your server by sending messages to Let's Encrypt's servers and verifying that the domain name(s) you've specified correspond to the correct IP address.

Install it via `yum`:
```bash
# sudo yum update
# sudo yum install certbot
```

# Run Certbot
In order to run Certbot, you'll need to down your `nginx`. Do it with:

```bash
# sudo service nginx restart
```

Decide which domains you'd like to generate SSL certificates for. For the following example, we use `example.com` with 2 additional sub-domains, `the.example.com` and `an.example.com`:

```bash
# sudo certbot certonly --standalone -d example.com -d the.example.com -d an.example.com
```

This will generate and place a certificate identified by `example.com` into `/etc/letsencrypt/live`.

# Add `nginx` Configuration
With the generated certificates, we proceed to modify the `nginx` server block corresponding to your website. The following is a server block for a website at `example.com` which should:

- redirect HTTP traffic to HTTPS 
- put logs in `/var/logs/example.com`
- belongs to user `u`

```nginx
server {
  listen 80;
  server_name example.com;
  return 301 https://afterwand.com$request_uri;
}

server {
  listen 443 ssl;
  server_name example.com;

  access_log /var/logs/$host.log;
  error_log /var/logs/$host.error.log;

  ssl on;
  ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
  ssl_prefer_server_ciphers on;

  location / {
    proxy_pass       http://127.0.0.1:8080;
    proxy_set_header Host    $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```

Have fun encrypting!
