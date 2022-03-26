# TD 4.1 - Chiffrement  

## Exercice 1 :  
1. `wget https://www.iut-amiens.fr/wp-content/uploads/BUT-INFO.pdf`  
2. `sha1sum BUT-INFO.pdf`  
bf5f69e6cd2f3cc105da79334559d4ff79a34f24  BUT-INFO.pdf  
3. C'est pas la même empreinte donc c'est pas le même fichier.  
4. `cd /var/www/html/liste`  
`touch dut2.txt`  
`nano dut2.txt`  
![](https://cdn.discordapp.com/attachments/939101409356431400/939104035586310154/unknown.png)  
5. `sha256sum dut2.txt > dut2.txt.sha256` pas sûr  

## Exercice 2 :  
`cd ~`  
1. `wget http://www.u13.org/chiffre.txt.aes128`  
2. `openssl aes128 -d -in chiffre.txt.aes128 -out chiffre.txt`  
Entrez le mdp : `BonjourDUT`  
`cat chiffre.txt`  
43  
3. `nano resultat.txt`  
![](https://cdn.discordapp.com/attachments/939101409356431400/939108453815812126/unknown.png)  
4. `openssl aes256 -in resultat.txt -out resultat.txt.aes256`  
Entrez le mdp : `BonjourDUT`  
5. `cp resultat.txt.aes256 /var/www/html/liste/`  
Salted__�^R�vN
F�c(s��/^N^L��sP�R�P
