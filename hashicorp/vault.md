## what is hashicorp vault
secrets/credentials management
authentication/authorization

functions:
- central secrets location
- fine-grained access control
- logs 
- short-lived dynamic credentials
- unique credentials per server/service
- key management and cryptography

## overview of high-level architecture
vault - 1. storage, 2. core 3 audit-log
client - eg. aws
secrets - kv, database, aws ,pki, ssh ( dynamic secrets with policy controlled)

multiple vault instances can share a common backend - and elect one leader node , to make HA

## certbot
```
sudo certbot certonly --standalone -d erminlc.mylabserver.com

## renew
certbot renew
```

##
initialize vault
```
systemctl stop vault
consul kv delete -rescurse vault/
systemctl start vault
vault operator init
# will return 5 unseal key, every time unseal need 3 of them, to reconstruct the master key
vault operator unseal <key>
```