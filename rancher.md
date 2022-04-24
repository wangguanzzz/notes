## rancher standalone upgrade
https://docs.pivotal.io/tanzu-buildpacks/stacks.html

## run rancher in standaloe in docker
```
docker run -d --restart=unless-stopped  --name rancher \
  -p 80:80 -p 443:443 \
  --privileged \
  rancher/rancher:latest
```

## how to run harbor from rancher
1. add repo https://helm.goharbor.io
2. must create a new cluster with ec2
3. in anwsers:
```
  tls:
    auto:
      commonName: <ip>
    certSource: auto
    enabled: true
    secret:
      notarySecretName: ''
      secretName: ''
  type: nodePort
externalURL: https://<ip>

## persistence 
false
```
4. access port https://<master-ip>:30003

replication from docker busybox is library/busybox