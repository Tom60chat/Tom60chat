# 25 - WebDav

1. Sur le site « ssl » proposer un répertoire webdav.

```bash
mkdir /var/www/webdav
chown www-data:www-data /var/www/webdav

a2enmod dav
a2enmod dav_fs

nano /etc/apache2/sites-available/default-ssl.conf
```

```apache
SecRuleEngine off
Alias /webdav /var/www/webdav

<Location /webdav>
    Options +Indexes
    DAV On
    AuthType Basic
    AuthName "webdav"
    AuthUserFile /var/www/.htwebdav
    Require valid-user
</Location>
```

2. Créer un utilisateur webdav/webdav qui pourra utiliser ce partage.

```bash
htpasswd -c /var/www/.htwebdav webdav

systemctl restart apache2
```

3. Monter ce partage depuis votre poste Windows/MacOS/Linux.

https://XXX.lprgi.u13.org/webdav