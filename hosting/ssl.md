

You can follow this <https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04>.

Or TL;DR

1. `# yum install certbot` | `# pacman -S certbot` | `# apt-get install letsencrypt`
2.  ```
location /.well-known {
	allow all;
	root /usr/share/nginx/html;
}
```
4. certbot --email email@example.com certonly -a webroot --webroot-path=/usr/share/nginx/html -d domain.io -d www.domain.io
5. ```
server {
	listen 80; 
	server_name  domain.io www.domain.io;
	rewrite ^/(.*) https://domain.io/$1 permanent;
}

server {
	listen       443 ssl;
	server_name  domain.io www.domain.io;

	ssl_certificate /etc/letsencrypt/live/domain.io/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/domain.io/privkey.pem;
```
6. sudo crontab -e

```
30 2 * * 1 /opt/letsencrypt/letsencrypt-auto renew >> /var/log/le-renew.log
35 2 * * 1 /usr/bin/systemctl reload nginx
```

NOTE: I do not know if `location /.well-known` is needed for the renewal script.

## Verify

```
https://www.ssllabs.com/ssltest/analyze.html?d=example.com&latest
```
