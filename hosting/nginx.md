# nginx

## Setup 1: 

1. Backup original `# mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.original`
2. `# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/nginx.conf -O /etc/nginx/nginx.conf`
3. `# mkdir /etc/nginx/sites-{available,enabled}`

## Setup 2.1: HTTP site

1. `# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/http.conf -O /etc/nginx/sites-available/example.conf`
2. `# ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/` 
3. `# systemctl restart nginx`

## Setup 2.2: HTTPS Site
 
## SSL

Replace step 4 above with the following steps

1. `# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/https.conf -O /etc/nginx/sites-available/example.conf`
2. Comment out lines 4 - 22 (<https://github.com/thebitmill/rc-vps/blob/master/https.conf#L4-L22>)
3. `# ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/` 
4. `# systemctl restart nginx`
5. `# certbot --email email@example.com certonly -a webroot --webroot-path=/usr/share/nginx/html -d domain.io -d www.domain.io`
6. Activate lines 4 - 22 again
7. `# systemctl restart nginx`
8. `# crontab -e` and insert `30 2 * * 1 /usr/bin/certbot renew --post-hook "/usr/bin/systemctl reload nginx" >> /var/log/le-renew.log`

## Troubleshooting

### SELinux permisison problems

```
# setsebool -P httpd_can_network_connect 1
```

