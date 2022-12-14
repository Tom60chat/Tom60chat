# 12 - Apache Avancé

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
   `a2enconf lprgi`
   `a2disconf lprgi`
9. `service apache2 restart`
   http://w2.53.lprgi.u13.org/
   Omg, des fichiers !