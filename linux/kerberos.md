everything in Kerberos is a principal , which is an identity can be asserted

## realm
Kerberos keeps track of pricinpals inside of waht we refer to as a **realm**
## key distribution center
KDC and a princiapl can communicate with symmetric cryptography

1. ticket
2. TGT
3. TGS

每个principal, 先发一个TGT去KDC证明登录，KDC去看数据库，然后回复给principal 一个TGT， principal把它放在cache里面用于auth 同一个realm 里面的principal ( 可能需要TGS)

commands
1. kinit (create a session)
2. klist (to see the service tickets)
3. kdestroy ( elimiate your TGT)
file:
/etc/krb5.conf


## use kerberos to authN server
1. server installation
```
yum install -y krb5-server krb5-workstation pam_krb5
```
edit /var/kerberos/krb5db/kdb.conf
edit /etc/krb5.conf
edit /var/kerberos/krb5kdc/kadm5.acl

create database
```
kdb5_util create -s -r MYLABSERVER.COM
systemctl enable krb5kdc kadmin
systemctl start krb5kdc kadmin
```

add pricinpals
```
kadmin.local
addprinc root/admin
# add user
addprinc krbtest

addprinc -randkey host/tco1.mylabserver.com
ktadd host/tco1.mylabserver.com
vim /etc/ssh/ssh_config
#uncomment
GGSAPI options

```
enable kerberos
```
authconfig --enablekrb5 --update
```

firewall
```
cd /etc/firewalld/services
vim kerberos.xml
## tpc 88 udp88 tcp 749
firewall-cmd --permanent --add-service=kerberos
firewall-cmd --reload
```

try login which local use
```
kinit 
# input passwd
ssh toc1.mylabesserver.com
```

## client side
```
yum install -y krb5-workstration pam_krb5
```
edit config, should be same as the krb server
```
vim /etc/krb5.conf
# important is to specify the realm
```

```
kadmin
addprinc -randkey host/tcox3.mylabserver.com
ktadd host/tcox3.mylabserv.com
```

edit the ssh like server
```
authconfig --enablekrb5 --update
```