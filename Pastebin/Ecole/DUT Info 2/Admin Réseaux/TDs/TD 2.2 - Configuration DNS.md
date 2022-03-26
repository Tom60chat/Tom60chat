# TD 2.2 - Configuration DNS
__Exercice 1 :__  
1 - `apt install bind9`  
2 - `nano /etc/hostname`  
![unknown](https://user-images.githubusercontent.com/25564492/160253062-2d29a2a3-6555-4d5f-b2bd-607994e3e736.png)  
3 - `nano /etc/hosts`  
![unknown](https://user-images.githubusercontent.com/25564492/160253071-6a6e96ee-049e-44a4-a8a5-a8f9c1953b7f.png)  
4 - `nano /etc/bind/named.conf.options`  
```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                10.1.139.230;
        };
        forward only;

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation no;

        listen-on port 53 { any; };
        listen-on-v6 port 53 { any; };

        allow-query { any; };

        allow-recursion { localhost; 10.1.136.0/22; };
};
```
5 - `systemctl restart bind9`  
6 - Comme la 4 ?  

__Exercice 2 :__ Serveur de zone  

1 - `ls -l /etc/resolv.conf`  
lrwxrwxrwx 1 root root 39 août  24 08:42 /etc/resolv.conf -> ../run/systemd/resolve/stub-resolv.conf  
`rm /etc/resolv.conf`  
`ls /run/systemd/resolve/`  
resolv.conf  
stub-resolv.conf  
`cp -a /run/systemd/resolve/resolv.conf /etc/resolv.conf`  
`nano /etc/resolv.conf`  
`chattr +i /etc/resolv.con`  
2 - `systemctl restart bind9`  

__Exercice 3__ : Serveur secondaire  
1 - `cd /etc/bind`  
`nano named.conf.local`  
![unknown](https://cdn.discordapp.com/attachments/530696349482483723/945714107968028692/unknown.png)  
2 - `systemctl restart bind9`  
`host www.dutxxx.iut-amiens.fr`  
```
www.dut16.iut-amiens.fr is an alias for tom.dut16.iut-amiens.fr.
tom.dut16.iut-amiens.fr has address 10.1.138.16
```
3 - `nano named.conf.local`  
![unknown](https://cdn.discordapp.com/attachments/935820454348865596/939126608994902026/unknown.png)  
`systemctl restart bind9`  
`host www.dutxxx.iut-amiens.fr 10.1.139.230`  
4 - `host test.iut-amiens.fr 10.1.139.230`  
`host test.iut-amiens.fr localhost`  
5 - `nano /var/cache/bind/dutxxx.iut-amiens.fr`  
![image](https://user-images.githubusercontent.com/25564492/160253143-6a9d29f8-7298-4b83-ae71-169fa1c8c0be.png)  
`systemctl restart bind9`  
`systemctl restart apache2`  
*Attendre un peu*  
http://www.dut16.iut-amiens.fr/ (Tester sur eduroam et VPN)  
