# 9 - Mysql

1. `apt install mariadb-server`
2. `systemctl start mariadb`
3. `systemctl enable mariadb`
4. `mysql -u root`
5. `SELECT Host,User,Password FROM mysql.user;`
6. `quit`
7. `/usr/bin/mysql_secure_installation`  
   *Spam la touche Entrée sans oublier les mdp*
8. `mysql -u root -p`
9. `SELECT Host,User,Password FROM mysql.user;`
10. `apt install phpmyadmin`
11. `cp /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpMyAdmin.conf`
12. `a2enconf phpMyAdmin`
13. `systemctl restart apache2`
14. `http://w1.53.lprgi.u13.org/phpmyadmin/`
    Username : phpmyadmin
    Mots de pass : mots de pass root