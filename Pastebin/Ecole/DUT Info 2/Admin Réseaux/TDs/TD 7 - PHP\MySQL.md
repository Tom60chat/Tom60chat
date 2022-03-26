# TD 7. PHP/MySQL (Pas vérifer)  
  
## Exercice 1  
  
1. `apt install php`  
2. `cd /var/www/html/`  
`nano essai1.php`  
```php
<?php
        phpinfo();
?>
```  
http://10.1.138.XXX/essai1.php  
  
`nano essai2.php`  
```php
<?php
        echo "<html><body>Hello World : ".posix_getuid()."</body></html>";
?>
```  
http://10.1.138.XXX/essai2.php  
  
`nano essai3.php`  
```php
<?php
        include("/etc/passwd");
?>
```  
http://10.1.138.XXX/essai3.php  
  
`nano essai4.php`  
```php
<?php
        $message="Bonjour,\r\nComment vas tu ?";
        $destinataire="root@localhost";
        $sujet="Hello";

        if(!mail($destinataire,$sujet,$message))
                echo "Erreur envoi de mail...";
        else
                echo "Mail OK";
?>
```  
http://10.1.138.XXX/essai4.php  
3. 4.​ 5.​ Déjà fait de base.  
6. `find /etc/php* -name "php.ini" -print`  
`nano /etc/php/7.4/apache2/php.ini`  
Ctrl + W et tapé "open_basedir" puis entrer.  
Retirez le ';' et ajouter /var/www  
![](https://cdn.discordapp.com/attachments/953598178920374313/953604056910270524/unknown.png)  
`systemctl restart apache2`  
7. http://10.1.138.XXX/essai3.php doit être vide  
## Ecercice 2  
1. `apt install mariadb-client`  
`apt install mariadb-server`  
2. `systemctl start mariadb`  
`systemctl enable mariadb`  
3. `/usr/bin/mysql_secure_installation`  
Mettre mdp root  
Y  
Mettre mdp root  
Remettre mdp root  
Spam Y  
4. `mysql -u root -p`  
5.  
```mysql
create database wp1;
```  
6.  
'Votre mdp' = votre mdp  
```mysql
create user 'wordpress'@'localhost' identified by 'Votre mdp';
grant all on wp1.* to wordpress@localhost identified by 'Votre mdp';
```  
  
```mysql
quit
```  
  
## Exercice 3
Toujours dans /var/www/html  
1. `wget https://fr.wordpress.org/latest-fr_FR.zip`  
2. `apt install unzip`  
`unzip latest-fr_FR.zip`  
`apt install php-mysql`  
`systemctl restart apache2`  
http://10.1.138.16/wordpress  
Cliquez sur "C'est parti !"  
![](https://cdn.discordapp.com/attachments/953598178920374313/953623742691561552/unknown.png)  
Copier le code préselctioner dans wp-config.pp  
`nano /var/www/html/wordpress/wp-config.php`  
et coller dans le fichier  
puis sur le site cliquer sur Lancer l'installation  
