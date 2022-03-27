# TD 6 : LDAP (Pas complet)  
	
## Exercice 1  
	
0. `apt install ldap-utils`  
1. `ldapsearch -h ldap.u-picardie.fr -LLL -b "ou=people,dc=u-picardie,dc=fr" -x '(uid=cd-iuta)'`  
2. `ldapsearch -h ldap.u-picardie.fr -LLL -b "ou=people,dc=etud,dc=u-picardie,dc=fr" -x '(n21012122)'`  

3. `ldapsearch -h ldap.u-picardie.fr -LLL -b "ou=people,dc=etud,dc=u-picardie,dc=fr" -x '(uid=#uidUPJV)'`  
Remplacer dans la commande #uidUPJV par le votre (ex: n21012122)	
4. `ldapsearch -h ldap.u-picardie.fr -LLL -b "ou=people,dc=etud,dc=u-picardie,dc=fr" -x '(uid=#uidUPJV)' -D 'uid=#uidUPJV,ou=people,dc=etud,dc=u-picardie,dc=fr' -W`  
5. Pareil, sauf qui faut mettre son mdp de l'UPJV  
6. `ldapsearch -h ldap.u-picardie.fr -LLL -b "ou=people,dc=u-picardie,dc=fr" -x '(sn=Dupont)'`	
7. `ldapsearch -h ldap.u-picardie.fr -LLL -b "ou=people,dc=u-picardie,dc=fr" -x '(givenName=Theo)'`	
	
## Exercice 2	
1. `apt install slapd`  
Taper votre mots de pass en boucle	
`dpkg-reconfigure slapd`  
![](https://cdn.discordapp.com/attachments/951109270978576424/951126595349930044/unknown.png)  
2. `nano arbo.ldif`
```
dn: ou=users,dc=dut16,dc=iut-amiens,dc=fr
objectClass: organizationalUnit
objectClass: top
ou: users

dn: ou=groups,dc=dut16,dc=iut-amiens,dc=fr
objectClass: organizationalUnit
ou: groups
```
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-amiens,dc=fr" -W -f arbo.ldif`  
3. `nano group.ldif`  
```dn: cn=ldapusers,ou=groups,dc=dut16,dc=iut-amiens,dc=fr
objectClass: posixGroup
cn: ldapusers
gidNumber: 5000
```
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-amiens,dc=fr" -W -f group.ldif`  
pour verif  
`ldapsearch -h localhost -LLL -b "ou=groups,dc=dut16,dc=iut-amiens,dc=fr" "(objectclass=*)" -x`  
4. `slappasswd`
```
New password: toutpuissant
Re-enter new password: toutpuissant
{SSHA}2XX/aL4izk9NAryg62ttSrFQbaICF/6k
```
`nano user.ldif`  
```bash
dn: uid=cd,ou=users,dc=dut16,dc=iut-amiens,dc=fr
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
loginShell: /bin/nologin
uid: cd
uidNumber: 5001
gidNumber: 5000
givenName: Bruce
sn: Wayne
cn: Bruce Wayne
homeDirectory: /sbin/nologin
displayName: Bruce Wayne
gecos: Bruce Wayne
shadowExpire: -1
shadowFlag: 0
shadowWarning: 7
shadowMin: 8
shadowMax: 999999
shadowLastChange: 10877
userPassword: {SSHA}2XX/aL4izk9NAryg62ttSrFQbaICF/6k
```
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-amiens,dc=fr" -W -f user.ldif`  
5. `slappasswd`
```
New password: jarvis
Re-enter new password: jarvis
{SSHA}To0HZIqqK3e2fKqGAChSC0obSrdCdcL6
```
`nano user2.ldif`
```bash
dn: uid=cd,ou=users,dc=dut16,dc=iut-amiens,dc=fr
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
loginShell: /bin/nologin
uid: cd
uidNumber: 5002
gidNumber: 5000
givenName: Tony
sn: Stark
cn: Tony Stark
homeDirectory: /bin/bash
displayName: Tony Stark
gecos: Tony Stark
shadowExpire: -1
shadowFlag: 0
shadowWarning: 7
shadowMin: 8
shadowMax: 999999
shadowLastChange: 10877
userPassword: {SSHA}To0HZIqqK3e2fKqGAChSC0obSrdCdcL6
```  
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-amiens,dc=fr" -W -f user2.ldif`  
6. `ldapsearch -h dut16.iut-amiens.fr -LLL -b "ou=people,dc=u-picardie,dc=fr" -x '(givenName=Tony)'`  

## Exercice 3  
1. `apt install sssd-ldap`  
2. `nano /etc/sssd/sssd.conf`  
```
[sssd]
services = nss, pam
config_file_version = 2
domains = dutXXX.iut-amiens.fr
[nss]
[domain/dutXXX.iut-amiens.fr]
id_provider = ldap
auth_provider = ldap
ldap_uri = ldap://10.1.138.XXX
cache_credentials = True
ldap_search_base = ou=users,dc=dutXXX,dc=iut-amiens,dc=fr
ldap_auth_disable_tls_never_use_in_production = true
```  
`chmod 600 /etc/sssd/sssd.conf`  
`pam-auth-update --enable mkhomedir`  
`systemctl restart sssd`  
`id bruce`  
  
Ne marche pas  
