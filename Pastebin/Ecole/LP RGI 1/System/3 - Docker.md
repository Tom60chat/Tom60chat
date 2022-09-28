El famouso : `sudo -s`

# Prérequis  
  
`apt update`  
`apt install docker.io`  
  
`systemctl start docker`  
`systemctl enable docker`  
  
`docker login -u tom60chat`  
  
`docker run hello-world`  
`docker pull alpine`

Vous devriez avoir ça à la fin :
![Docker images](https://user-images.githubusercontent.com/25564492/192715689-7fa95d16-2fa0-42d5-bdef-6e62c3632ec9.png)

# Cours (Vous pouvez ignorer cette partie)
  
![image](https://user-images.githubusercontent.com/25564492/192716402-e0e449b2-d35a-4b7f-b3d2-db60fb39f367.png)

`docker create -i -t --name alp1 alpine`

`docker run -d alpine /bin/sh -c "while true; do echo hello world; sleep 1; done"`

![image](https://user-images.githubusercontent.com/25564492/192719914-0d005821-817c-4696-9870-5c128e763796.png)

![image](https://user-images.githubusercontent.com/25564492/192721461-a694e348-4367-479b-ada9-05feb2fd53e5.png)

![image](https://user-images.githubusercontent.com/25564492/192722957-329b0bb9-a5a6-4f5d-9642-549e3800a41b.png)

# je sais pas trop là mais faut le faire je crois même si c'est pas dans le TD

`docker pull httpd`

`docker run -d -p 8080:80 --rm --name Apache httpd`

![image](https://user-images.githubusercontent.com/25564492/192730077-bd2664d0-007b-440f-892b-51cb0510bba7.png)

`docker network create mynet1`

![image](https://user-images.githubusercontent.com/25564492/192728810-1d54569b-ab4c-4f2e-9785-a8b285039775.png)

`docker run -t -d --rm --name a1-mynet1 --network mynet1 alpine`

`docker exec a1-mynet1 hostname -i`  
![image](https://user-images.githubusercontent.com/25564492/192730343-48f4e5f5-e168-430b-bf3f-61484e106b35.png)

`docker run -t -d --rm --name a2-mynet1 --network mynet1 alpine`

`docker exec a2-mynet1 hostname -i`  
![image](https://user-images.githubusercontent.com/25564492/192730383-556ce633-b03d-4fef-8f72-459753a6ffc8.png)

`docker exec a2-mynet1 ping -c 3 a1-mynet1`  
![image](https://user-images.githubusercontent.com/25564492/192730263-3709cb91-d484-4da5-8367-82fda90fe56e.png)

`docker volume create web1`

`docker volume inspect web1`
![image](https://user-images.githubusercontent.com/25564492/192731786-fc401f90-89cc-4452-9588-cbcf6bb8f7a2.png)

`docker run -d -p 8081:80 --name=dock1 -v web1:/usr/local/apache2/htdocs httpd`

`docker inspect dock1`

`nano /var/lib/docker/volumes/web2/_data/index.htm`  
![image](https://user-images.githubusercontent.com/25564492/192730704-91e367b7-dcf3-48b6-95e9-c40a10884d7e.png)

`docker run -d -p 8082:80 --name=dock2 -v web2:/usr/local/apache2/htdocs httpd`

`nano /var/tmp/apache/index.html`

`docker run -d -p 8083:80 --name=dock3 -v /var/tmp/apache:/usr/local/apache2/htdocs httpd`

# TD1 - Docker

```
docker search wordpress
docker pull wordpress:latest
docker search mariadb
docker pull mariadb:latest

docker network create rezorgi
docker volume create vol6_bd

docker run -d --name=dock6-bd -v vol6_bd:/var/lib/mysql --network rezorgi -e MYSQL_ROOT_PASSWORD=4321 -e MYSQL_DATABASE=bdd -e MYSQL_USER=bdd -e MYSQL_PASSWORD=4321 --rm mariadb

docker run -d -p 3101:80 --name=dock6-wp -v vol6_bd:/var/lib/mysql --network rezorgi -e WORDPRESS_DB_HOST=dock6-bd:3306 -e WORDPRESS_DB_USER=bdd -e WORDPRESS_DB_PASSWORD=4321 -e WORDPRESS_DB_NAME=bdd --rm wordpress
```

a un moment il vous seras demandé de créer un script c'est ce qu'il faudras rentrer dedans je précise que ce qu'il y a entre [] est a modifié pour vous c'est le mot de passe etc pour la bdd vous mettez ce que vous voulez mais ça doit correspondre entre mariadb et wordpress !

Si jamais vous rencontrez des problèmes avec la connexion de base de donnée wordpress vous devez faire attention que MYSQL_USER et WORDPRESS_DB_USER soit les même ainsi que MYSQL_PASSWORD et WORDPRESS_DB_PASSWORD

Une fois que vous aurez tout fait vous pourrez accéder a votre wordpress pour cela il faudras ouvrir un CMD a coté
taper la commande :
ssh -L 3101:10.1.137.[num]:3101 [utilisateur]@10.1.137.[num]
et ensuite vous pourrez accéder a ce site
http://localhost:3101/
