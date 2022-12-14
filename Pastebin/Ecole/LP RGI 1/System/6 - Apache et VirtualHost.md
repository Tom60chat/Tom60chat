# 6 - Apache et VirtualHost

1. ?? (C'est ce qu'ont fait à la 5.)
2. `mkdir /var/www/vhosts`
3. `mkdir /var/www/vhosts/w1.XX.lprgi.u13.org`
   `mkdir /var/www/vhosts/w2.XX.lprgi.u13.org`
4. `mkdir /var/www/vhosts/w1.XX.lprgi.u13.org/html`
   `mkdir /var/www/vhosts/w2.XX.lprgi.u13.org/html`
   `mkdir /var/www/vhosts/w1.XX.lprgi.u13.org/log`
   `mkdir /var/www/vhosts/w2.XX.lprgi.u13.org/log`
5. `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`
   ```apache
   <VirtualHost *:80>
       ServerName w1.XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w1.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w1.XX.lprgi.u13.org/log/access_log combined
   </VirtualHost>
   ```
   `nano /etc/apache2/sites-available/w2.XX.lprgi.u13.org.conf`
   ```apache
   <VirtualHost *:80>
       ServerName w2.XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w2.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w2.XX.lprgi.u13.org/log/access_log combined
   </VirtualHost>
   ```
6. `a2ensite w1.XX.lprgi.u13.org w2.XX.lprgi.u13.org`
   `systemctl reload apache2`
7. `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`
   ```apache
   <VirtualHost *:80>
       ServerName w1.XX.lprgi.u13.org
       ServerAlias XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w1.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w1.XX.lprgi.u13.org/log/access_log combined
   </VirtualHost>
   ```