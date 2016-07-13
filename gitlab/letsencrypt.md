# Lets encrypt

Followed <http://stackoverflow.com/a/34539809/4273291>

But instead of creating `letsencrypt-config/gitlab.ini` and 
`/etc/cron.monthly/renew-ssl-certificates` i used auto-renew.

I then put the following in my `crontab -e`, i have not verified this works yet

```
30 2 * * 1 /opt/letsencrypt/letsencrypt-auto renew >> /var/log/le-renew.log
35 2 * * 1 /usr/bin/gitlab-ctl restart nginx
```
