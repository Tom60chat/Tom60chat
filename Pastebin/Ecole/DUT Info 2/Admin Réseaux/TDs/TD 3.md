# TD 3  

__Exercice 1__ :  
1 - `apt install apache2`  
2 - `apachectl -v`  
3 - `systemctl start apache2 & systemctl enable apache2`  
4 - http://10.1.138.X/  
5 - `nc -C localhost 80`  
Server: Apache  
6 - `curl -I localhost`  
**Server:** Apache/2.4.41 (Ubuntu)  
7 - `nano /etc/apache2/conf-enabled/security.conf`  
![](https://cdn.discordapp.com/attachments/935810711102296064/935816509886971904/unknown.png)  
8 - `systemctl restart apache2 & curl -I localhost`  
**Server:** Apache  
9 - `cd /var/www/html/`  
`mkdir liste`  
`cd liste/`  
`touch file1.txt file2.txt`  
`ls -l`  
![](https://cdn.discordapp.com/attachments/935810711102296064/935820419829727262/unknown.png)  
10 - `touch /etc/apache2/conf-available/dut.conf`  
`nano /etc/apache2/conf-available/dut.conf`  
![](https://cdn.discordapp.com/attachments/935810711102296064/935822629024190494/unknown.png)  
11 - `a2enconf`  
`dut.conf` et appuyer sur entrée  
`systemctl restart apache2`  
`systemctl status apache2` -> Pour vérifier qu'il n'y a aucune erreur  
![](https://cdn.discordapp.com/attachments/935810711102296064/935823026937794600/unknown.png)  
Puis retourner sur le site pour vérifier que le forbidden apparaît bien  

__Exercice 2__ :  
1 - Il en à un de base  
2 - `cd /var/www/html`  
`mkdir private`  
`cd private`  
`htpasswd -c .htpasswd sam`  
```
New password: sam
Re-type new password: sam
Adding password for user sam
```
`nano /etc/apache2/sites-enabled/000-default.conf`  
```
        <Directory /var/www/html/private>
                AuthType Basic
                AuthName "Acces Restreint"
                AuthUserFile /var/www/.htpasswd
                Require user sam luke
        </Directory>
```  
![](https://cdn.discordapp.com/attachments/935810711102296064/957296293674373120/unknown.png)   
3 - `touch index.html`  
4 - http://www.dut16.iut-amiens.fr/liste => Forbidden  
5 - `nano /etc/apache2/sites-enabled/000-default.conf`  
Ajouter après `<Directory /var/www/html/private>`  
...  
```
        <Directory /var/www/html/liste>
                Options +Indexes
        </Directory>
```
6 - `cd /var/www/html/`  
`mkdir liste2`  
http://10.1.138.x/liste2/ => Forbiden  
7 - `nano /etc/apache2/sites-enabled/000-default.conf`  
![](https://cdn.discordapp.com/attachments/935810711102296064/935838310109151232/unknown.png)  
`systemctl restart apache2`  
http://10.1.138.X/liste/ => Dossier liste  
```
Alias /inti/ "/home/inti"
```

__Exercice 3__ :  
1 - `nano /etc/apache2/sites-enabled/000-default.conf`   
```
        Redirect /iut https://www.iut-amiens.fr/
```
![](https://cdn.discordapp.com/attachments/935810711102296064/957297454003408936/unknown.png)  
`systemctl restart apache2`  
http://www.dut16.iut-amiens.fr/iut  
