# 22 - Proxy Avancé


## Exercice 1

1. Installer SQUID
```bash
apt install squid
```
2. Ajoutez une règle pour interdire l'accès au site facebook.com.
```bash
nano /etc/squid/conf.d/facebook.conf
```
```conf
acl facebook dstdomain .facebook.com
http_access deny facebook
```

Tester :
- curl --proxy http://10.1.137.XXX:3128 https://www.facebook.com
- curl --proxy http://10.1.137.XXX:3128 https://www.google.com
