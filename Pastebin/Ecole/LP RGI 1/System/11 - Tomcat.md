# 11 - Tomcat

1. `apt install tomcat9 tomcat9-admin`
2. `systemctl enable tomcat9`
   `systemctl start tomcat9`
   http://localhost:8080/ (Faire le tunnel ssh)
3. `nano /etc/apache2/sites-available/tomcat.XXX.lprgi.u13.org.conf`
   ```apache
   <VirtualHost *:80>
       ServerName tomcat.XXX.lprgi.u13.org
       ProxyPreserveHost On
       ProxyPass / ajp://localhost:8009/
   </VirtualHost>
   ```
   `nano /etc/tomcat9/server.xml`
   ![server.xml](Images/11%20-%20Tomcat%20(9).png)
   ```xml  
   <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" address="0.0.0.0" secretRequired="false" />
   ```
4. `nano /etc/tomcat9/tomcat-users.xml`
   ```xml
   ...
     <role rolename="admin"/>
     <role rolename="admin-gui"/>
     <role rolename="manager"/>
     <role rolename="manager-gui"/>
     <role rolename="manager-status"/>
     <user username="admin" password="admin" roles="admin,admin-gui"/>
     <user username="manager" password="manager" roles="manager,manager-gui,manager-status"/>
   ...
   </tomcat-users>
   ```
   `systemctl restart tomcat9`
   `a2ensite tomcat.XXX.lprgi.u13.org.conf`
   `a2enmod proxy_ajp`
   `systemctl restart apache2`
   http://tomcat.XXX.lprgi.u13.org/manager/status
5. `cd /var/lib/tomcat9/webapps/ROOT/`
   `wget http://www.u13.org/ressources/tomcat/ess.jsp`
   http://tomcat.XXX.lprgi.u13.org/ess.jsp
6. `ls /var/cache/tomcat9/Catalina/localhost/ROOT/org/apache/jsp/`
7. `cd /var/lib/tomcat9/webapps`
   `wget www.u13.org/ressources/tomcat/hello.war`
   http://tomcat.XXX.lprgi.u13.org/hello
8. Télécharger : http://www.u13.org/ressources/tomcat/admin.war
   `http://tomcat.XXX.lprgi.u13.org/manager/`
   ![WAR file to deploy](Images/11%20-%20Tomcat%20WAR.png)
   Choisizer le admin.war que vous avez téléchargez, puis "Deploy".
   https://tomcat.XXX.lprgi.u13.org/admin  
   Utiliser l'utilisateur admin et mdp admin
   
9. `nano /etc/apache2/sites-available/w1.XXX.lprgi.u13.org.conf`
    ```apache
    <VirtualHost *:80>
        ...

        ProxyPass /hello ajp://localhost:8009/hello/
        ProxyPass /manager ajp://localhost:8009/manager/
        ProxyPass /admin ajp://localhost:8009/admin/
    </VirtualHost>
    ```
    `a2dissite w1.XXX.lprgi.u13.org`
    `a2ensite w1.XXX.lprgi.u13.org`
    `service apache2 restart`
    http://XXX.lprgi.u13.org/admin
