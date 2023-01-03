# 12 bis - hors TD

1. `a2dissite w1.XX.lprgi.u13.org`
    `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`
    ```apache
    <VirtualHost *:80>
        ServerName www.XX.lprgi.u13.org
        #ServerAlias XX.lprgi.u13.org
        ...
    ```
    `mv /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf /etc/apache2/sites-available/www.XX.lprgi.u13.org.conf`
2. `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`
    ```apache
    <VirtualHost *:80>
        ServerName w1.XX.lprgi.u13.org
        Redirect / http://www.XX.lprgi.u13.org/
    </VirtualHost>
    ```
3. `a2ensite www.XX.lprgi.u13.org w1.XX.lprgi.u13.org && systemctl reload apache2`
