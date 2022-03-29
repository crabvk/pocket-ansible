# Obtain SSL certificate with certbot

> More info about certbot tool: https://certbot.eff.org  
> More info about Let's Encrypt certificate authority: https://letsencrypt.org

Install certbot and nginx packages on your server.  
Set Nginx configuration */etc/nginx/nginx.conf* to:

```nginx
events {}

http {
    server {
        listen 80 default_server;
        try_files $uri index.html;

        location /.well-known/acme-challenge/ {
            root /usr/share/nginx/html;
        }
    }
}
```

NOTE: prepend each of the following commands with `sudo` if your current user isn't root.

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

Obtain certificate with certbot:

```shell
certbot certonly --webroot -w /usr/share/nginx/html -d $domain -n --agree-tos -m $email
```
