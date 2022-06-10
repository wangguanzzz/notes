## rule of thumb
check nginx doc, go to https://nginx.org
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

# basic nginx security
## generate self-signed certificate
```
mkdir /etc/nginx/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/private.key -out /etc/nginx/ssl/public.pem
```

## configure the host for TLS/SSL
```
server {
  listen 80 default_server;
  listen 443 ssl;
  ssl_certificate /etc/nginx/ssl/public.pem;
  ssl_certificate_key /etc/nginx/ssl/private.key;
  server_name _;
  root /var/www/html;
}
```
```
curl -k https://localhost
```

## location module
```
server {
  ...
  locaton = /admin.html {
    auth_basic "Login required";
    auth_basic_user_file /etc/nginx/.htpasswd;
  }
}
```
## http rewrite module

```
server {
  ...
  rewrite ^(/.*).html(\?.*)?$ $1$2 redirect; 
  # /admin.html?debug=true
  # first capture group =?  /admin  second=> ?debug=true
}

```

try_files 避免不需要被redirect的

# nginx module

## overview of module
modules subdirectory contains the dynamic module
```
# show the entire configuration
nginx -V
nginx -V 2>&1 | tr -- - '\n' | grep _module
```

load_module is used to load dynamic module
```
load_module modules/ngx_mail_module.so
```

# reverse proxy

## reverse proxy with proxy_pass
use ngx_http_proxy_module  (proxy_pass)
proxy_set_header can update the header

```
server {
  ...
  location / {
    # to enable keep alive
    proxy_http_version 1.1;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;
    # for web socket
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    proxy_pass http://127.0.0.1:3000;
  }

  #for static file, ~* tilde * ignore upper/lower case
  location ~* \.(js|css|png|jpe?g|gif) {
    root /var/www/photos.example.com;
  }
}
```

## setup LEMP stack
fastCGI is a binary protocal let server/client talk to each other