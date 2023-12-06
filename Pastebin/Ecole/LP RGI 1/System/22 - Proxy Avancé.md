# 22 - Proxy Avancé


## Exercice 1

1. Installer SQUID
```bash
apt install squid
```
2. Ajoutez une règle pour interdire l'accès au site facebook.com.
```bash
nano /etc/squid/conf.d/facebook.conf
```
```conf
acl facebook dstdomain .facebook.com
http_access allow localnet !facebook
```
```bash
systemctl restart squid
```

Tester :
- curl --proxy http://10.1.138.XXX:3128 https://www.facebook.com
- curl --proxy http://10.1.138.XXX:3128 https://www.google.com

## Exercice 2

1. Créez un serveur Web en utilisant un conteneur Docker en passant par un docker-compose et la commande "docker stack".

```yaml
version: "3.9"

services:
  web:
    image: httpd
    volumes:
      - ./www:/usr/local/apache2/htdocs
    ports:
      - 80:80
```

```bash
docker stack deploy -c docker-compose.yml web
```

2. Vous exporterez la racine des pages Web via un volume docker accessible en local.

```bash
mkdir www
echo "Hello World" > www/index.html
```

```bash
curl http://localhost
```

3. Créez un virtualhost Apache basé sur le nom "pm.XX.lprgi.u13.org" qui sera un reverse proxy vers le conteneur précédant mais en utilisant uniquement les règles de réécritures (pas de ProxyPass ni de ProxyPassReverse).

4. Sur ce site pm, ajouter une règle de réécriture pour interdire les accès aux fichiers ".txt", testez en créant un fichier test.txt à la racine du site.

5. Créez également un fichier nommé "test.abc" dans lequel vous mettrez du texte aléatoire, vérifiez que ce fichier est lui accessible.

## Exercice 3

1. Sur le conteneur précédent, vous allez créer la page "index.html" suivante :
```html	
<html><body>
<img src="http://localhost/image.jpg"><br />
Bonjour
</body></html>
```

2. Vous placerez une image nommée "image.jpg" également dans la racine du site, testez, conclusion ?

3. Sur le site proxy, essayez d'utiliser la directive ProxyHtmlUrlMap pour résoudre le problèmes des adresses IPs de la Ubuntu.