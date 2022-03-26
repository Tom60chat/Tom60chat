# TD 6 : LDAP (Pas vérifier, complet)  
	
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
![](https://cdn.discordapp.com/attachments/951109270978576424/951129070505164800/unknown.png)  
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-amiens,dc=fr" -W -f arbo.ldif`  
3. `nano group.ldif` 
![](https://cdn.discordapp.com/attachments/951109270978576424/951130447029612645/unknown.png)  
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-a
miens,dc=fr" -W -f group.ldif`  
pour verif  
`ldapsearch -h localhost -LLL -b "ou=group
s,dc=dut16,dc=iut-amiens,dc=fr" "(objectclass=*)" -x`  
4. `nano user.ldif`  
![](https://cdn.discordapp.com/attachments/951109270978576424/951132276224315502/unknown.png)  
`ldapadd -x -D "cn=admin,dc=dut16,dc=iut-a
miens,dc=fr" -W -f user.ldif`  

à suivre...
