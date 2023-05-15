# 20 - Sécurité CGI

1. Configurez les sites w1 et w2 pour que les CGI standards utilisent respectivement les utilisateurs httpw1 et httpw2 à l'aide du module suexec
```bash
# Si vous ne les avait pas déjà créé
groupadd "httpw1"
groupadd "httpw2"

apt install apache2-suexec-custom
a2enmod suexec

nano /etc/apache2/sites-available/www.53.lprgi.u13.org.conf
```
```apache
<VirtualHost *:80>
    ServerName www.XXX.lprgi.u13.org
    ...
    SuexecUserGroup httpw1 httpw1

    <Directory
    ...
</VirtualHost>
```
```bash
nano /etc/apache2/sites-available/w2.XXX.lprgi.u13.org.conf
```
```apache
<VirtualHost *:80>
    ServerName w2.XXX.lprgi.u13.org
    ...
    SuexecUserGroup httpw2 httpw2
    
   <Directory 
   ...
</VirtualHost>
```
```bash
sudo chown -R httpw1.httpw1 /var/www/vhosts/w1.XXX.lprgi.u13.org/cgi-bin/
sudo chown -R httpw2.httpw2 /var/www/vhosts/w2.XXX.lprgi.u13.org/cgi-bin/

systemctl restart apache2
```

2. Pour PHP laissez le site w1 en fonctionnement standard c'est à dire qui va utiliser le module PHP.
Rien faire ducoup.

3. Pour le PHP du site w2, vous activerez l'exécution de php-cgi comme indiqué dans le cours.
```bash
a2enmod actions
apt install php-cgi
sudo chown -R httpw1.httpw1 /var/www/vhosts/w1.53.lprgi.u13.org/cgi-bin/


nano /etc/apache2/sites-available/w2.XXX.lprgi.u13.org.conf
```
```apache
<VirtualHost *:80>
    ServerName w2.XXX.lprgi.u13.org
    ...

    SuexecUserGroup httpw2 httpw2

    php_admin_flag engine off
    <FilesMatch \.php$>
        SetHandler none
    </FilesMatch>

   <Directory "/var/www/vhosts/w2.53.lprgi.u13.org/html/">
        # On active les CGI
        Options +ExecCGI
        # Les CGI doivent avoir pour extension .cgi ou .pl
        AddHandler cgi-script .cgi .pl .sh .bin
        AddHandler php-script .php
        Action php-script /cgi-bin/php-cgi
    </Directory>
    ...
</VirtualHost>
```
```bash
systemctl restart apache2

cp /usr/bin/php-cgi /var/www/vhosts/w2.53.lprgi.u13.org/cgi-bin/
/bin/nano /var/www/vhosts/w2.53.lprgi.u13.org/html/essai1.php
```
```php
#!/usr/bin/php

<?php
    echo "<html><body>Hello World : ".posix_getuid()."</body></html>";
?>  
```
```bash
chown -R httpw2.httpw2 /var/www/vhosts/w2.53.lprgi.u13.org/cgi-bin/
chown -R httpw2.httpw2 /var/www/vhosts/w2.53.lprgi.u13.org/html/
chmod +X /var/www/vhosts/w2.53.lprgi.u13.org/cgi-bin/php-cgi
chmod +X /var/www/vhosts/w2.53.lprgi.u13.org/html/essai1.php
```



4. Installez le paquet php-fpm et configurez le comme dans le cours avec la directive "listen 127.0.0.1:9000" plutôt que le socket.

```bash	
apt install php-fpm
a2enmod proxy_fcgi setenvif
systemctl restart php7.4-fpm

nano /etc/php/7.4/fpm/pool.d/www.conf
```
```apache	
[www]
...
;listen = /run/php/php7.4-fpm.sock
listen = 127.0.0.1:9000
...
```
```bash
systemctl restart php7.4-fpm
```

5. Vous allez configurez maintenant un site w3 en vous basant sur le site w2 qui va utiliser php-fpm, testez avec le script essai2.php.

```bash
cp -a /etc/apache2/sites-available/w2.XXX.lprgi.u13.org.conf /etc/apache2/sites-available/w3.XXX.lprgi.u13.org.conf
```
6. Ajoutez un utilisateur httpw3 dont les caractéristiques seront identiques aux utilisateurs httpw1 et httpw2.
```apache
useradd -c "httpw3" -d /var/www/vhosts/w3.XXX.lprgi.u13.org -s /usr/sbin/nologin httpw3
```
7. Vous allez maintenant créer une configuration de php-fpm spécifique pour w3 qui écoutera sur le port 9003 et s'éxécutera avec l'utilisateur httpw3,
```bash
cp -a /etc/php/7.4/fpm/pool.d/www.conf /etc/php/7.4/fpm/pool.d/httpw3.conf
nano httpw3.conf
```
```apache
;[www]
[httpw3]
...
;user = www-data
user = httpw3
...
;listen = 127.0.0.1:9000
listen = 127.0.0.1:9003
...
;group = www-data
group = httpw3
...
```
8. Configurez le virtualhost apache de w3 pour qu'il utilise ce service, vérifiez avec le script essai2.php.
```bash
nano /etc/apache2/sites-available/w3.XXX.lprgi.u13.org.conf
```
```apache
<VirtualHost *:80>
    ServerName w3.XXX.lprgi.u13.org
    CustomLog /var/www/vhosts/w3.53.lprgi.u13.org/log/access_log combined
    ...
    php_admin_flag engine off
    <FilesMatch \.php$>
        SetHandler "proxy:fcgi://127.0.0.1:9000"
    </FilesMatch>
    ...
</VirtualHost>
```
```bash
service apache2 restart
service php7.4-fpm restart
```
http://w3.XXX.lprgi.u13.org/essai1.php