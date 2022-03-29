# Your Pocket node domain name and DNS record

## Domain name

It is required for a Pocket node to be accessible via HTTPS, thus you need an SSL certificate.
Though, an SSL certificate can be issued to an IP address, according to [this answer](https://stackoverflow.com/a/1119269/1878180), it is rarely used.

You can buy a domain name from a domain registrar. To name a few:

* [dynadot](https://www.dynadot.com/)
* [porkbun](https://porkbun.com/)
* [namecheap](https://www.namecheap.com/)

I currenty use and recommend porkbun, they have low prices, great service and easy to use web interface.

## DNS record

In order to bind your domain name to your Pocket server's IPv4 address you need a DNS record type A.  
Here's [instruction for porkbun](https://kb.porkbun.com/article/54-how-to-use-a-records-to-point-your-domain-at-a-web-host), but you can easily find similar instruction for your domain registrar. They all have the same principle.

It is advised to add a subdomain, for example node1.pokt.cryptoved.com. This way you can create more subdomains in the future and host more stuff, like a web site or another Pocket node, within the same domain.
