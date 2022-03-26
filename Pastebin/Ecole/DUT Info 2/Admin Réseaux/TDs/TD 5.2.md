# TD 5.2 (Pas vérifier/ complet)

## Exercice 1 :

1. `apt install postfix`  
Fleche du bas, entrée  
![](https://cdn.discordapp.com/attachments/948598901106151495/948599390858276944/unknown.png)  
![](https://cdn.discordapp.com/attachments/948598901106151495/948599560035500092/unknown.png)  
![](https://cdn.discordapp.com/attachments/948598901106151495/948599645997772892/unknown.png)  
2. `apt install mailutil`  
3.  `mail root`  
Ctrl + D pour envoyer  
4. `mail`  
![](https://cdn.discordapp.com/attachments/948598901106151495/948601335312425011/unknown.png)  
5. `nano /etc/aliases`  
![](https://cdn.discordapp.com/attachments/948598901106151495/948607349499756635/unknown.png)  
`newaliases`  
`mail tous`  
Ctrl + D  
`mail`  
6. `nc localhost 25`  
```bash
MAIL FROM:<root>
RCPT TO:<root>
DATA
Body of email.
.
QUIT
EOF
```  
7.
```
MAIL FROM:<drocourt@iut-amiens.fr>
RCPT TO:<root>
DATA
Subject: Test
Ceci est un test
.
QUIT
EOF
```
8. `apt install swaks`  
`swaks --to root --server localhost`  
9. `apt install dovecot-core dovecot-imapd dovecot-pop3d`  
`systemctl start dovecot`  
`nano /etc/dovecot/dovecot.conf`  
![](https://cdn.discordapp.com/attachments/948598901106151495/948610843623424010/unknown.png)  
`nano /etc/dovecot/conf.d/10-mail.conf`  
![](https://cdn.discordapp.com/attachments/948598901106151495/948611372151873637/unknown.png)  
normalement il y a rien à faire, la ligne est déjà là  
10. `nc localhost 110`  
```
USER tom
PASS tom
RETR 1
QUIT
EOF
```  
11.`nc localhost 143`  
```
a001 LOGIN tom tom
a002 select inbox
a003 FETCH 1 FULL
a004 logout
EOF
```  

## Exercice 2 :
1. `host iut-amiens.fr`  
2. ​3. ​4.​  
 `nano /var/cache/bind/dut16.iut-amiens.fr`  
![](https://cdn.discordapp.com/attachments/948598901106151495/948617833040994344/unknown.png)  
`host -t MX dut16.iut-amiens.fr`  
`systemctl restart bind9`  
`host -t MX dut16.iut-amiens.fr`  
5. Pas envie de télécharger Thunderbird  
VPN : https://pedag.u-picardie.fr/moodle/upjv/course/view.php?id=9496  
Adresse de courier : tom@dutxxx.iut-amiens.fr  
Nom d'utilisateur : tom  
Mot de passe : mdp  
Entrant (IMAP) : www.dutxxx.iut-amiens.fr:143  
Sortant (SMTP) : www.dutxxx.iut-amiens.fr:25  
Require l'authetification  
Ne pas mettre SSL  
