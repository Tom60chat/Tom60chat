# TD 1 - Rappels
1. Créer un groupe nommé "stagiaires" :
```bash
sudo groupadd stagiaires
getent group stagiaires
```
2. Créez un utilisateur nommé "Indiana Jones" avec le login "indi" :
```bash
sudo useradd -c "Indiana Jones" -s /bin/bash -m indi
getent passwd inti
```
3. Créez un utilisateur nommé "The Big Lebowski" avec le login "dude" :
```bash
sudo useradd -c "The Big Lebowski" -s /bin/bash -m dude
getent passwd dude
```
4. Ajoutez les deux utilisateurs précédents au groupe stagiaire :
```bash
sudo usermod -G stagiaires indi
sudo usermod -G stagiaires dude
getent group stagiaires
```
5. Positionnez des mots de passe pour les deux utilisateurs précédents et assurez vous qu'ils doivent le changer à la première connexion :
```bash
sudo passwd indi
sudo passwd dude
sudo passwd -e indi
sudo passwd -e dude
sudo grep indi /etc/shadow
sudo grep dude /etc/shadow
```
6. Créer un utilisateur pour vous, avec en commentaire (GECOS) la valeur NOM Prénom, vous utiliserez un login ne contenant que des minuscules et potentiellement des chiffres :
```bash
sudo useradd -c "OLIVIER Tom" -s /bin/bash -m tom
getent passwd MonMotsDePass
```
7. Positionner un mot de passe pour votre utilisateur :
```bash
sudo passwd tom
```
8. Ajouter votre utilisateur au groupe "sudo" :
```bash
sudo usermod -G sudo tom
getent group tom
su tom
sudo cat /etc/shadow
exit
```
9. Vous déconnecter et vous reconnecter à votre serveur en utilisant votre utilisateur :
```bash
exit
ssh tom@10.1.138.16
```
10. Verrouiller le compte "inti" et vérifier :
```bash
sudo passwd -l inti
```
11. Stopper le service "ufw" :
```bash
sudo systemctl status ufw
sudo systemctl stop ufw
```
12. Désactiver le service "ufw" au démarrage :
```bash
sudo systemctl diseable ufw
```
13. Avec la commande "apt", installez l'application "finger" et testez la commande avec votre login :
```bash
sudo apt install finger
finger
```
