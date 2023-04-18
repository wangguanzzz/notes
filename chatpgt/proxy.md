1. spin-up a centos 8 server
2.  install WARP
```
bash <(curl -fsSL git.io/warp.sh) proxy
```
3.  install ansible-core
```
yum install -y ansible-core
```
4. scp ok_proxy.zip to host and unzip it
5. ./install_v2ray.sh localhost
6. scp  config.json  root@<ip>:/etc/v2ray/config.json
7. systemctl restart v2ray