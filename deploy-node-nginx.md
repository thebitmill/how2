# Deploy A Node App With Nginx on CentOS

```
# useradd liverpooltravel -m
# su liverpootravel
$ cd ~
$ ssh-keygen
```

TODO example nginx.conf

```
server {
    listen 80; 
    server_name  anderssonmotorsport.nu www.anderssonmotorsport.nu;
    rewrite ^/(.*) https://anderssonmotorsport.nu/$1 permanent;
}

server {
    listen       443 ssl;
    server_name  anderssonmotorsport.nu www.anderssonmotorsport.nu;

    ssl_certificate /etc/letsencrypt/live/anderssonmotorsport.nu/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/anderssonmotorsport.nu/privkey.pem;

    root /home/ams/ams/public;
    index index.html;

    # for lets encrypt (--webroot)
    location /.well-known {
        allow all;
        root /usr/share/nginx/html;
    }

    location / {
        proxy_redirect      off;
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto   $scheme;
        proxy_set_header    Host                $http_host;
        proxy_set_header    X-NginX-Proxy       true;
        proxy_set_header    Connection          "";
        proxy_http_version  1.1;
        #proxy_cache    one;
        #proxy_cache_key    sfs$request_uri$scheme;
        proxy_pass  http://ams;
    }

    #======Do not serve files starting with . or $=======
    #location ~ /\. { access_log off; log_not_found off; deny all; }
    location ~ ~$  { access_log off; log_not_found off; deny all; }

}
```


Copy output from last command to deploy key

```
$ git clone git@repo.url
```





