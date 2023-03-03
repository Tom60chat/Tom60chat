1. `openssl aes256 -in fichier.txt -out fichier.crypt`
2. `openssl sha1 fichier.txt`
3. ![Certificats](./Images/14%20-%20cert.png)
4. `openssl s_client -connect www.drocourt.info:443`
5. `a2enmod ssl`
6. `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ssl.XXX.lprgi.u13.org.pem -out ssl.XXX.lprgi.u13.org.crt`
   `cp ./ssl.XXX.lprgi.u13.org.crt /etc/ssl/certs/`
   `cp ./ssl.XXX.lprgi.u13.org.pem /etc/ssl/private/`
7. `nano /etc/apache2/sites-available/lprgi.conf`
   ```apache
   ...
   SSLCertificateFile /etc/ssl/certs/ssl.XXX.lprgi.u13.org.crt
   SSLCertificateKeyFile /etc/ssl/private/ssl.XXX.lprgi.u13.org.key
   ```
a2ensite default-ssl

nouveau virtual host
