---
description: Installation d'un certificat auto-signé (https) OpenSSL avec nginx.
categories: [GNU/Linux]
date: 2021-10-22
tags: [GNU/Linux, debian, openssl, ssl, tls, chiffrement, https]
visibility: public
order: 46
---

# :key: Installation d'un certificat auto-signé OpenSSL avec nginx

==- :icon-unverified: Un problème avec sudo ?

!!!
Si vous configurez votre serveur directement en root. N'oubliez pas de retirer le `sudo` de chaque commande.
!!!

!!! danger
:arrows_counterclockwise: Si vous avez mis un mot de passe au compte root, la commande `sudo` ne sera pas acceptée.
Connectez-vous directement en root pour exécuter les commandes.  
:arrow_right: Vous pouvez aussi, réinstaller votre système en laissant le mot de passe root vide, lors de l'installation.  
`sudo` s'installera et fonctionnera correctement.
!!!

==-

---

## :closed_lock_with_key: Générer le certificat et la clé

!!!success :icon-repo-locked: Recommendation sécurité
Il est recommandé d'utiliser `ed25519` qui correspond au standard actuel en terme de sécurité.  
Toutefois il n'est pas compatible partout, surtout sur les systèmes legacy.  
Dans ce cas `RSA` est utilisé, il est préférable de mettre une longueur de clé conséquente.
!!!

```
sudo openssl req -x509 -days 365 -out mycert.crt -nodes -newkey rsa:4096 -keyout mykey.key
```

:icon-shield-lock: Renseigner les différentes informations du certificat

```
Generating a RSA private key
..................................+++++
........................................................+++++
writing new private key to 'mykey.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:FR
State or Province Name (full name) [Some-State]:RHONE
Locality Name (eg, city) []:maville
Organization Name (eg, company) [Internet Widgits Pty Ltd]:CONTACTIT
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:web.it.fr
Email Address []:contactit.yarka@slmail.me
```

!!! danger
`Common Name` doit être renseigné le nom de domaine !
!!!

!!!
OpenSSL a généré le certificat et la clé dans le répertoire ou vous êtes au moment ou vous avez tappé la commande.  
Vous pouvez les déplacer/renommer comme bon vous semble.
!!!


---

## :memo: Éditer le fichier de configuration nginx

```# sudo nano /etc/nginx/sites-enabled/default
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;
        location / {
                try_files $uri $uri/ =404;
        }
}
```

:icon-chevron-right: en haut du fichier se trouve un **block de configuration server**.  
Il écoute sur le port `80:HTTP` qui pointe vers `/var/www/html` puis une page d'accueil définie dans `index` ligne 5.

:icon-chevron-right: Comme nous allons **ajouter un block de configuration server** qui écoute sur le port `443:HTTPS`, il nous faut le changer pour éviter des **conflits**:  

```#
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name _;
        return 301 https://$host$request_uri;
}
```

:icon-chevron-right: `Ligne 5` Redirige les requêtes du **port 80** sur le **port 443**.  

:icon-file-symlink-file: Ajouter la configuration SSL à la fin du fichier

```# /etc/nginx/sites-enabled/default
server {
        listen 443 ssl;
        server_name _;
        root /var/www/html;
        ssl_certificate /var/www/sites/client1/mycert.crt;
        ssl_certificate_key /var/www/sites/client1/mykey.key;
        index index.html index.htm index.nginx-debian.html;
}
```

:icon-chevron-right: `Ligne 4` Remplacer le chemin par le votre.  
:icon-chevron-right: `Ligne 5-6` Remplacer les chemins par les votres.  

:icon-arrow-right: Le fichier de configuration ressemble alors à ça (sans les commentaires):  

```cat /etc/nginx/sites-enabled/default
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name _;
        return 301 https://$host$request_uri;
}

server {
        listen 443 ssl;
        server_name _;
        root /var/www/html;
        ssl_certificate /var/www/sites/client1/mycert.crt;
        ssl_certificate_key /var/www/sites/client1/mykey.key;
        index index.html index.htm index.nginx-debian.html;
}
```

---

## 🔄 Redémarrer nginx:

```
sudo service nginx restart
```

!!! success
Vous pouvez maintenant consulter votre site web avec votre certificat auto-signé (https://monsite.local par exemple).
!!!

---

:icon-diff-renamed: Dernière modification: 19/09/2022 - 14h00


