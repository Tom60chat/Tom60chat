# TD 4.2 - Certificats et TLS (Pas complet)

1. `a2enmod ssl`  
`systemctl restart apache2`  
2. `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout www.dut16.iut-amiens.fr.pem -out www.dut16.iut-amiens.fr.crt  
Generating a RSA private key`  
![](https://cdn.discordapp.com/attachments/945701857127452692/945721245448683540/unknown.png)  
3. `cp ./www.dut16.iut-amiens.fr.crt /etc/ssl/certs/`  
`cp ./www.dut16.iut-amiens.fr.pem /etc/ssl/private/`  
4. `nano /etc/apache2/sites-available/default-ssl.conf`  
Metre en commentaire le SSCertificate  
![](https://cdn.discordapp.com/attachments/945701857127452692/945722764940177449/unknown.png)  
5. `openssl s_client -connect www.dutxxx.iut-amiens.fr:443`  

## ça marche pas après (Help wanted)

6. `nano /etc/bind/named.conf.local`  
Ajouter  
```
zone "ssl.dut16.iut-amiens.fr" {
 type slave;
 masters {10.1.139.230;};
};
```  
7. `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ssl.dut16.iut-amiens.fr.pem -out ssl.dut16.iut-amiens.fr.crt`  
8. `cp ./ssl.dut16.iut-amiens.fr.crt /etc/ssl/certs/`  
`cp ./ssl.dut16.iut-amiens.fr.pem /etc/ssl/private/`  
9. `cp default-ssl.conf iut-ssl.conf`  
`nano iut-ssl.conf`  
```
        <VirtualHost _default_:443>
                ServerName ssl.dutXXX.iut-amiens.fr
                ...
                SSLCertificateFile /etc/ssl/certs/ssl.dut16.iut-amiens.fr.crt
                SSLCertificateKeyFile /etc/ssl/private/ssl.dut16.iut-amiens.fr.key
                ...
        </VirtualHost>
```  
`a2ensite`  
iut-ssl  
`systemctl reload apache2`  
10 - `cd /var/www/ssl`  
touch index.html  
11 - https://ssl.dut16.iut-amiens.fr/  
