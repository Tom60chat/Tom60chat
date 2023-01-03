# 12 - Apache Avancé

## Exercice 1
1. `cd /var/www/vhosts/w1.XXX.lprgi.u13.org/html/`
   `mkdir liste`
2. http://w1.XXX.lprgi.u13.org/liste/  
   Omg, un dossier vide !
3. `cd /etc/apache2/conf-available`
4. `nano lprgi.conf`
   ```apache
   <Directory "/var/www">
    Options -Indexes
   </Directory>
   ```
   `a2enconf lprgi`
5. `service apache2 restart`
   http://w1.XXX.lprgi.u13.org/liste/  
   Omg, fourbeinguedeun !
6. `cd /var/www/vhosts/w2.XXX.lprgi.u13.org/html/`
   `mkdir liste`
   `cd liste`
   `touch file1.txt file2.txt`
7. http://w2.53.lprgi.u13.org/liste/
   Omg, forbiden !
8. `nano /etc/apache2/conf-available/lprgi.conf`
   ```apache
   ...
   <Directory "/var/www/vhosts/w2.XXX.lprgi.u13.org/html/liste">
    Options +Indexes
   </Directory>
   ```
   `a2disconf lprgi`
   `a2enconf lprgi`
9. `service apache2 restart`
   http://w2.53.lprgi.u13.org/
   Omg, des fichiers !

## Exercice 2

1. `mkdir /var/www/vhosts/w1.XX.lprgi.u13.org/html/private/`  
   `echo Bonjour > /var/www/vhosts/w1.XX.lprgi.u13.org/html/private/index.html`  
   `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`  
   ```apache
   <Directory "/var/www/vhosts/w1.XX.lprgi.u13.org/html/private/">
     AuthType Basic
     AuthName "Acces Restrint"
     AuthUserFile /var/www/.htpasswd
     Require user sam luke
   </Directory>
   ```  
   `a2dissite w1.XX.lprgi.u13.org && a2ensite w1.XX.lprgi.u13.org && systemctl restart apache2`  

   `htpasswd -cB /var/www/.htpasswd sam`  
   ```bash
   New password: sam
   Re-type new password: sam
   ```  
   `htpasswd -B /var/www/.htpasswd luke`  
   ```bash
   New password: luke
   Re-type new password: luke
   ```  
   http://w1.XX.lprgi.u13.org/private/
2. `mkdir /var/www/vhosts/w2.XX.lprgi.u13.org/html/private/`  
   `echo Bonjour > /var/www/vhosts/w2.XX.lprgi.u13.org/html/private/index.html`  
   `nano /var/www/.htgroup`  
   ```
   lprgi: sam luke
   ```  
   `nano /etc/apache2/sites-available/w2.XX.lprgi.u13.org.conf`  
   ```apache
   <Directory "/var/www/vhosts/w2.XX.lprgi.u13.org/html/private/">
     AllowOverride AuthConfig
   </Directory>
   ```  
   `a2dissite w2.XX.lprgi.u13.org && a2ensite w2.XX.lprgi.u13.org`  
   `nano /var/www/vhosts/w2.XX.lprgi.u13.org/html/private/.htaccess`  
   ```apache
   AuthType Basic
   AuthName "Acces Restrint"
   AuthUserFile /var/www/.htpasswd
   AuthGroupFile /var/www/.htgroup
   Require group lprgi
   ```  
   `a2enmod authz_groupfile && systemctl restart apache2`  
   http://w2.XX.lprgi.u13.org/private/