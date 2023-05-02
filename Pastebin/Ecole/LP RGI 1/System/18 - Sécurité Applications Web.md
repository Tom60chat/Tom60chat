# 18 - Sécurité Applications Web

1. Configurer un nouveau VirtualHost pour ce nom DNS qui sera en fait un reverse proxy vers le site « www.u13.org »,
```bash
nano /etc/apache2/sites-available/proxy.XXX.lprgi.u13.org.conf
```
```apache
<VirtualHost *:80>
    ServerName proxy.XXX.lprgi.u13.org
    ProxyPass / http://www.u13.org/
    ProxyPassReverse / http://www.u13.org/
</VirtualHost>
```

2. Installer le module Apache « mod_security »
```bash
apt install -y libapache2-mod-security2
cd /etc/modsecurity/
mv modsecurity.conf-recommended modsecurity.conf
nano modsecurity.conf
```
```apache
# -- Rule engine initialization ----------------------------------------------

# Enable ModSecurity, attaching it to every transaction. Use detection
# only to start with, because that minimises the chances of post-installation
# disruption.
#
SecRuleEngine On
...
```
```bash
systemctl restart apache2
```

3. Testez les différentes attaques réalisées précédemment en passant par ce site
http://proxy.XXX.lprgi.u13.org/tempor/
http://proxy.XXX.lprgi.u13.org/tempor/ch4/xss/?toto=%3Cscript%3Econsole.log(hello)%3C/script%3E

4. Vérifiez également que vos autres sites fonctionnent encore correctement
http://www.XXX.lprgi.u13.org
http://w1.XXX.lprgi.u13.org
http://w2.XXX.lprgi.u13.org
http://joomla.XXX.lprgi.u13.org
https://ssl.XXX.lprgi.u13.org
http://tomcat.XXX.lprgi.u13.org
http://wp.XXX.lprgi.u13.org


