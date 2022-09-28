# TD
 

2. Créez vous un utilisateur membre du groupe "sudo" et positionnez un mot de passe  

`useradd -c "Cyril Drocourt" -s /bin/bash -G sudo -m cd`  
`passwd cd`  
`sudo passwd -l inti`  

1. Modifiez le nom de votre serveur  

`sudo nano /etc/hostname`  
![image](https://user-images.githubusercontent.com/25564492/188652907-1cc437a0-927d-4ec9-92d9-4c27eb240dfc.png)  
`ip -br a`  
![image](https://user-images.githubusercontent.com/25564492/188652640-a306eaaa-c084-437e-985d-42e125f6d988.png)  
`sudo nano /etc/hosts`  
![image](https://user-images.githubusercontent.com/25564492/188652345-7fc34791-43da-4d1c-a3b8-b44c2833c3c7.png) 

?. Apache ?  
`sudo apt update`  
`sudo apt install apache2`  
`sudo systemctl enable apache2`  
`sudo systemctl start apache2`  
