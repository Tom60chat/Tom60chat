# 10 - Proxy
Vous allez ajouter un reverse proxy associé au nom wp.XX.lprgi.u13.org permettant l'accès à l'application wordpress contenu dans le docker géré par Swarm.

1. `a2enmod proxy_html`
2. `a2enmod proxy_http`
3. `nano /etc/apache2/sites-available/wp.XX.lprgi.u13.org.conf`
    ```apache
    <Virtualhost *:80>
     serveradmin webmaster@wp.XX.lprgi.u13.org
     servername wp.XX.lprgi.u13.org
     ProxyRequests off
     ProxyPass / http://localhost:3101/
     ProxyPassReverse / http://localhost:3101/
     # Si on veut garder le nom demandé (hostname)
     ProxyPreserveHost On
    </Virtualhost>
   ```
4. `systemctl reload apache2`



