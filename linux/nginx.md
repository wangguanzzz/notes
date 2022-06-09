## understand the default config
/etc/nginx/nginx.

```
# check if configuration syntax is OK
nginx -t 
```

# simple vitual host

Nginx 可以serve多个域名
default, in default.conf
```
server {
  listen 80 default_server;
  server_name _;
  root /var/www/html;
}

```

example.com in example.com.conf
```
server {
  listen 80;
  server_name example.com www.example.com;
  root /var/www/example.com/html;
}

```

```
# use curl to test , add header as a domain name
curl --header "Host: example.com" localhost
```