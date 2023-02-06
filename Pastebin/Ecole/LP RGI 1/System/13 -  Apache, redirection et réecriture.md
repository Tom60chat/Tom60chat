## 13 -  Apache, redirection et réecriture

1.  - `a2dissite w1.XX.lprgi.u13.org`
      `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`
      ```apache
      <VirtualHost *:80>
          ServerName www.XX.lprgi.u13.org
          #ServerAlias XX.lprgi.u13.org
          ...
      ```
      `mv /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf /etc/apache2/sites-available/www.XX.lprgi.u13.org.conf`
    - `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`
      ```apache
      <VirtualHost *:80>
          ServerName w1.XX.lprgi.u13.org
          Redirect / http://www.XX.lprgi.u13.org/
      </VirtualHost>
      ```
    - `a2ensite www.XX.lprgi.u13.org w1.XX.lprgi.u13.org && systemctl reload apache2`
    http://w1.XX.lprgi.u13.org/ -> http://www.XX.lprgi.u13.org

2. `mkdir /var/www/downloads`

3. `nano /etc/apache2/conf-available/lprgi.conf`
   ```apache
   ...
   <Directory "/var/www/downloads/">
     Options +Indexes
   </Directory>

   Alias /Downloads/ /var/www/downloads/
   ```
   http://www.53.lprgi.u13.org/Downloads/

4. `echo "😺 Les chats c'est la vie !" > /var/www/downloads/README.md`

5. `sudo a2enmod rewrite`
   `nano /etc/apache2/conf-available/lprgi.conf`
   ```apache
   ...
   # Fonctionne pas
   RewriteEngine on
   RewriteRule ^(.*)\.html$ $1.html [L]
   ```


