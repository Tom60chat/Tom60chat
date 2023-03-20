# 14 - Chiffrement

1. `openssl aes256 -in fichier.txt -out fichier.crypt`

2. `openssl sha1 fichier.txt`

3. ![Certificats](./Images/14%20-%20cert.png)


4. `openssl s_client -connect www.drocourt.info:443`
5. `a2enmod ssl

6. `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ssl.XXX.lprgi.u13.org.pem -out ssl.XXX.lprgi.u13.org.crt`
   CommonName: ssl.XXX.lprgi.u13.org
   `cp ./ssl.*.lprgi.u13.org.crt /etc/ssl/certs/`
   `cp ./ssl.*.lprgi.u13.org.pem /etc/ssl/private/`

7. `cp www.XXX.lprgi.u13.org.conf ssl.XXX.lprgi.u13.org`
   `nano /etc/apache2/sites-available/ssl.XXX.lprgi.u13.org.conf`
   ```apache
   <VirtualHost *:443>
      ServerName ssl.XXX.lprgi.u13.org

      ...

      SSLEngine on
      SSLCertificateFile      /etc/ssl/certs/ssl.XXX.lprgi.u13.org.crt
      SSLCertificateKeyFile   /etc/ssl/private/ssl.XXX.lprgi.u13.org.pem

      ...
   ```
   `a2ensite ssl.XXX.lprgi.u13.org`
   `sudo systemctl restart apache2`

8. https://ssl.XXX.lprgi.u13.org/

9. `sudo mkdir -r /var/www/vhosts/ssl.53.lprgi.u13.org/html`
   `nano /etc/apache2/sites-available/ssl.53.lprgi.u13.org.conf`
   ```apache
      ...

      DocumentRoot /var/www/vhosts/ssl.XXX.lprgi.u13.org/html

      ...
   ```

10. `nano /etc/apache2/sites-available/ssl.XX.lprgi.u13.org.conf`
    ```apache
      <VirtualHost *:80>
         ServerName ssl.XXX.lprgi.u13.org

         # Redirection 301 vers le site en HTTPS
         Redirect permanent / https://ssl.XXX.lprgi.u13.org/
      </VirtualHost>

      ...
    ```
    `sudo systemctl restart apache2`

11. `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/certs/www.XXX.lprgi.u13.org.pem -out /etc/ssl/certs/www.XXX.lprgi.u13.org.crt`
   CommonName: www.XXX.lprgi.u13.org

12. Les deux fonctionne

13. Car j'ai du modifier defaul-ssl.conf je crois
   `nano /etc/apache2/sites-available/default-ssl.conf`
   ```apache
      SSLCertificateFile      /etc/ssl/certs/www.53.lprgi.u13.org.crt
      SSLCertificateKeyFile   /etc/ssl/private/www.53.lprgi.u13.org.pem
   ```
