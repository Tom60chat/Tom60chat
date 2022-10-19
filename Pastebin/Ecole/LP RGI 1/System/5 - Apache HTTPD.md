# 5 - Apache HTTPD

1. `apt install apache2` (Déjà fait)
2. `apachectl -t`
3. `nano /etc/httpd/conf/httpd.conf`
   ```apache
   ```
   `apachectl -t`
4. `tail /var/log/apache2/error.log`
5. `mv /var/www/html/index.html /var/www/html/index.html.old`
   `nano /var/www/html/index.html`
   ```html
   <html><body>Super</body></html>
   ```