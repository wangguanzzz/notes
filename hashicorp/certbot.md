## install of cerbot on ubuntu
```
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install certbot
```

## create the cert
```
sudo certbot certonly --standalone -d <domain name>
```

