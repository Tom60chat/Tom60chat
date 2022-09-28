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

# je sais pas trop là

`docker pull httpd`

`docker run -d -p 8080:80 --rm --name Apache httpd`

`curl -i http://localhost:8080`
