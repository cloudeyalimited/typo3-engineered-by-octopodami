# TYPO3 Engineered by OCTOPODAMI (TEBO) SSL

You can secure your Octopodami installation with an SSL certificate by following the instructions below.

+ Log in to your domain registrar.
+ Update your domain `A` or `CNAME` record to point to the EC2 Instance IP Address.
+ Wait for 15-20 minutes so DNS can propagate.
+ Log in to the EC2 instance via the terminal.
+ Set your domain name in the Nginx configuration and reload the service:

```sh
sudo sed -i 's/server_name _;/server_name mydomainname.com;/' /etc/nginx/conf.d/typo3.conf
sudo nginx -t && sudo systemctl reload nginx
```

+ Install Certbot and request the certificate:

```sh
sudo dnf install -y certbot python3-certbot-nginx
sudo certbot --nginx -d mydomainname.com
## Answer all the questions (this is a comment)
## Then, specify your domain name (this is a comment)
```

+ Set the TYPO3 site base URL to your domain:

```sh
sudo sed -i "s|^\([[:space:]]*base:[[:space:]]*\).*$|\1'https://mydomainname.com/'|" /var/www/typo3/config/sites/*/config.yaml
```

+ Stop the boot service from resetting the base URL to the public IP:

```sh
sudo systemctl disable update-instance-id.service
```

+ Test the renewal:

```sh
sudo certbot renew --dry-run
```

+ Access your domain name (`https://mydomainname.com`) and unlock the site with the instance ID as usual.

TYPO3 writes absolute links from the configured base URL, so the site base change matters after you add a domain. The instance ID stays valid, so the unlock page still works over HTTPS.

## Links

1. [Marketplace Listing](https://aws.amazon.com/marketplace/pp/prodview-gy3tue2fp74ec)
2. [EULA](./octopodamiEULA.txt)
3. [Knowledgebase](https://github.com/cloudeyalimited/typo3-engineered-by-octopodami/-/wikis/home)
4. [Issue Tracking](https://github.com/cloudeyalimited/typo3-engineered-by-octopodami/-/issues)
5. [Changelog](./changelog.md)

## Support

[Email](mailto:tech@cloudeya.org) support is available to Amazon Web Services Marketplace Customers. We do not offer refunds, but you may terminate your TYPO3 Engineered by OCTOPODAMI (TEBO) Stack at any time.

## License

The documentation is published under [BSD 3-Clause License](license.txt).

## Copyright

(c) 2020 - 2026 [Cloudeya Limited](https://cloudeya.org).
