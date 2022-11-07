# 5 - Apache HTTPD

1. `apt install apache2` (Déjà fait)
2. `apachectl -t`
3. `nano /etc/apache2/conf-enabled/security.conf`
   ```apache
   # ServerTokens
   # This directive configures what you return as the Server HTTP response
   # Header. The default is 'Full' which sends information about the OS-Type
   # and compiled in modules.
   # Set to one of:  Full | OS | Minimal | Minor | Major | Prod
   # where Full conveys the most information, and Prod the least.
   #ServerTokens Minimal
   #ServerTokens OS
   #ServerTokens Full
   ServerTokens Prod
   ```
   `apachectl -t`
4. `tail /var/log/apache2/error.log`
5. `mv /var/www/html/index.html /var/www/html/index.html.old`
   `nano /var/www/html/index.html`
   ```html
   <html><body>Super</body></html>
   ```