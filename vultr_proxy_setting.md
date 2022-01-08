## vultr proxy
https://www.vultr.com

1. buy a centos server in seatle
2. setup server
```console
bash <( curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/centos_install_v2ray.sh )
```
3. choose the port [60888]

in client, update the outbound part
```json
 "outbounds": [{
    "protocol": "vmess",
    "settings": {
      "vnext": [{
        "address": "137.220.34.236", 
        "port": 60888,
        "users": [{ "id": "27d4ac63-7ae3-4a5c-854c-5620941796c4" }]
      }]
    }
  },{
```
