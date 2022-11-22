# Joomla

1. `mkdir /var/www/vhosts/joomla.XX.lprgi.u13.org`
2. `mkdir /var/www/vhosts/joomla.XX.lprgi.u13.org/html`
3. `mkdir /var/www/vhosts/joomla.XX.lprgi.u13.org/log`
4. `nano /etc/apache2/sites-available/joomla.XX.lprgi.u13.org.conf`
    ```apache
    <VirtualHost *:80>
    ServerName joomla.XX.lprgi.u13.org
    DocumentRoot /var/www/vhosts/joomla.XX.lprgi.u13.org/html
    CustomLog /var/www/vhosts/joomla.XX.lprgi.u13.org/log/access_log combined
    </VirtualHost>
    ```
5. `a2ensite joomla.XX.lprgi.u13.org`
6. `systemctl reload apache2`
7. `wget https://downloads.joomla.org/cms/joomla4/4-0-3/Joomla_4-0-3-Stable-Full_Package.zip`
8. `apt install unzip`
9. `unzip Joomla_4-0-3-Stable-Full_Package.zip -d /var/www/vhosts/joomla.XX.lprgi.u13.org/html/`
10. `chown -R www-data: /var/www/vhosts/joomla.XX.lprgi.u13.org/html/`
11. `mysql -u root -p`
12. ```sql
    CREATE DATABASE joomla;
    GRANT ALL PRIVILEGES ON joomla.* TO 'joomla'@'localhost' IDENTIFIED BY  'StrongPassword';
    FLUSH PRIVILEGES;
    EXIT;
    ```
13. http://joomla.XX.lprgi.u13.org/
    Nom d'utilisateur base de données : joomla
    Mot de passe base de données : StrongPassword
    Nom de la base de données : joomla