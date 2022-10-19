El famouso : `sudo -s`

# Prérequis  
  
`apt update`  
`apt install docker.io`  
  
`systemctl start docker`  
`systemctl enable docker`  
  
`docker login -u tom60chat`  v
  
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

# Entrainement je pense (toujours pas le TD)

`docker pull httpd`

`docker run -d -p 8080:80 --rm --name Apache httpd`

`curl -i http://localhost:8080`  
![image](https://user-images.githubusercontent.com/25564492/192730077-bd2664d0-007b-440f-892b-51cb0510bba7.png)

`docker network create mynet1`

`docker network list`  
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
...

`ls -l /var/lib/docker/volumes/web1/_data`

`docker volume create web2`

`nano /var/lib/docker/volumes/web2/_data/index.html`
```html
<html><body>Super</body></html>
```

`docker run -d -p 8082:80 --name=dock2 -v web2:/usr/local/apache2/htdocs httpd`

`docker inspect dock2`

`curl http://localhost:8082`  
![img.png](Images/img1.png)

# TD1 - Docker

1. `docker pull wordpress:latest`

2. `docker pull mariadb:latest`

3. `docker network create rezorgi`

4. `docker volume create vol6_bd`

5. `docker run -d --name=dock6-bd -v vol6_bd:/var/lib/mysql --network rezorgi -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=bdd -e MYSQL_USER=user -e MYSQL_PASSWORD=password --rm mariadb`

6. `docker ps`

7. Les ports ...

8. `docker run -d -p 3101:80 --name=dock6-wp -v vol6_bd:/var/lib/mysql --network rezorgi -e WORDPRESS_DB_HOST=dock6-bd:3306 -e WORDPRESS_DB_USER=user -e WORDPRESS_DB_PASSWORD=password -e WORDPRESS_DB_NAME=bdd --rm wordpress`

9. `docker ps`

10. `docker logs [id]`

11. `nano /root/td31.sh`
   ```
   docker pull wordpress:latest
   docker pull mariadb:latest

   docker network create rezorgi
   docker volume create vol6_bd

   docker run -d --name=dock6-bd -v vol6_bd:/var/lib/mysql --network rezorgi -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=bdd -e MYSQL_USER=user -e MYSQL_PASSWORD=password --rm mariadb
   docker run -d -p 3101:80 --name=dock6-wp -v vol6_bd:/var/lib/mysql --network rezorgi -e WORDPRESS_DB_HOST=dock6-bd:3306 -e WORDPRESS_DB_USER=user -e WORDPRESS_DB_PASSWORD=password -e WORDPRESS_DB_NAME=bdd --rm wordpress
   ```

Pour ce connecter au serveur, vous pouvez avec putty :   
![img.png](Images/img2.png)

Ensuite commencer l'installation de WordPress :
http://localhost:3101
![img.png](Images/img3.png)
Puis "Installer WordPress"
