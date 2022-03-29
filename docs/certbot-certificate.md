# Obtain SSL certificate with certbot

> More info about certbot tool: https://certbot.eff.org  
> More info about Let's Encrypt certificate authority: https://letsencrypt.org

Install certbot and nginx packages on your server.  
Set Nginx configuration */etc/nginx/nginx.conf* to:

```nginx
events {}

http {
    error_log syslog:server=unix:/dev/log;
    access_log syslog:server=unix:/dev/log;

    server {
        listen 80 default_server;
        try_files $uri index.html;

        location /.well-known/acme-challenge/ {
            root /usr/share/nginx/html;
        }
    }
}
```

> NOTE: prepend each of the following commands with `sudo` if your current user isn't root.

Restart Nginx:

```shell
systemctl restart nginx
```

Make sure Nginx is running by looking at the logs:

```shell
journalctl -fu nginx
```

Set Pocket node's full domain name and your email for certificate renewal notifications:

```shell
domain=node1.pokt.example.com
email=name@example.com
```

Obtain a certificate with certbot:

```shell
certbot certonly --webroot -w /usr/share/nginx/html -d $domain -n --agree-tos -m $email
```

> NOTE: There is a Failed Validation limit of 5 failures per account, per hostname, per hour.
> If you get "too many failed authorizations recently" error from the certbot, wait for one hour or choose another subdomain and try again.
