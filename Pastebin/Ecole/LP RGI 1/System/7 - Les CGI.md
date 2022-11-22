# 7 - Les CGI

1. `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`  
   ```apache
   <VirtualHost *:80>
       ServerName w1.XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w1.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w1.XX.lprgi.u13.org/log/access_log combined
       ScriptAlias /cgi-bin/ /var/www/vhosts/w1.XX.lprgi.u13.org/cgi-bin/
   </VirtualHost>
   ```  
   `nano /etc/apache2/sites-available/w2.XX.lprgi.u13.org.conf`  
   ```apache
   <VirtualHost *:80>
       ServerName w2.XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w2.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w2.XX.lprgi.u13.org/log/access_log combined
       ScriptAlias /cgi-bin/ /var/www/vhosts/w2.XX.lprgi.u13.org/cgi-bin/
   </VirtualHost>
   ```  

   `mkdir /cgi-bin/`  
   `cd /cgi-bin/`  

2. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai1.pl`  
   ```perl
   #!/usr/bin/perl
   print "Content-type: text/html\n\n";
   print "Bonjour a tous !!!";
   ```  
   `chmod a+x /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai1.pl`  
   `/var/www/vhosts/w1.XX.lprgi.u13.org/html/essai1.pl`  
3. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai2.sh`  
   ```bash
   #!/bin/bash
   echo "Content-type: text/html"
   echo ""
   echo "<html><body>"
   echo "Bonjour<br /><pre>"
   /bin/env
   echo "</pre></body></html>"
   ```  
   `chmod a+x /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai2.sh`  
   `/var/www/vhosts/w1.XX.lprgi.u13.org/html/essai2.sh`  
4. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai3.c`  
   ```c
   #include <stdio.h> 
   #include <stdlib.h>
   int main () {
       printf("Content-type: text/html\n\n");
       printf("<html><body>Bonjour\n");
       printf("</body></html>");
       exit(0);
   }
   ```  
   `apt install gcc`  
   `cc /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai3.c -o /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai3.bin`  
   `/var/www/vhosts/w1.XX.lprgi.u13.org/html/essai3.bin`  
   `cp /var/www/vhosts/w1.XX.lprgi.u13.org/html/* /var/www/vhosts/w2.XX.lprgi.u13.org/cgi-bin/`  
5. `a2enmod cgi`  
   `systemctl reload apache2`  
6. `nano /etc/apache2/sites-available/w1.XX.lprgi.u13.org.conf`  
   ```apache
   <VirtualHost *:80>
       ServerName w1.XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w1.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w1.XX.lprgi.u13.org/log/access_log combined
       ScriptAlias /cgi-bin/ /var/www/vhosts/w1.XX.lprgi.u13.org/cgi-bin/
   
       <Directory "/var/www/vhosts/w1.XX.lprgi.u13.org/cgi-bin/"> 
           # On active les CGI
           Options +ExecCGI
           # Les CGI doivent avoir pour extension .cgi ou .pl
           AddHandler cgi-script .cgi .pl .sh .bin
       </Directory>
   </VirtualHost>
   ```
   `nano /etc/apache2/sites-available/w2.XX.lprgi.u13.org.conf`
   ```apache
   <VirtualHost *:80>
       ServerName w2.XX.lprgi.u13.org
       DocumentRoot /var/www/vhosts/w2.XX.lprgi.u13.org/html
       CustomLog /var/www/vhosts/w2.XX.lprgi.u13.org/log/access_log combined
       ScriptAlias /cgi-bin/ /var/www/vhosts/w2.XX.lprgi.u13.org/cgi-bin/
   
       <Directory "/var/www/vhosts/w2.XX.lprgi.u13.org/html/"> 
           # On active les CGI
           Options +ExecCGI
           # Les CGI doivent avoir pour extension .cgi ou .pl
           AddHandler cgi-script .cgi .pl .sh .bin
       </Directory>
   </VirtualHost>
   ```
   `systemctl reload apache2`

   http://w1.XX.lprgi.u13.org/cgi-bin/essai1.pl
   http://w2.XX.lprgi.u13.org/essai1.pl