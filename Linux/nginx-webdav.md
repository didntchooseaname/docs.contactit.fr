---
description: Installation de Nginx (serveur web) + Webdav (fichiers sur http) sur debian 11.
categories: [GNU/Linux, nginx]
date: 2021-10-21
tags: [GNU/Linux, debian, nginx, webdav]
order: 47
---

# :ng: Installation de Nginx + Webdav sur debian 11

![](images/nginx.webp)

!!! Warning Cette documentation est en cours d'écriture :writing_hand:
Il pourrait y avoir quelques erreures.  
Si vous en remarquez une, contactez-moi [ici](mailto:contactit.yarka@slmail.me) :slightly_smiling_face:
!!!

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

## :ng: Installation de Nginx

:icon-file-submodule: Pour avoir les modules nécessaires, il faut installer nginx dans sa version complète:

```
sudo apt update && sudo apt install nginx-full
```

:icon-check-circle: Ca y'est ! C'est fini 😪, votre site est accessible dans votre navigateur à l'adresse **http://IP_de_votre_machine**

==- :icon-check-circle: Configuration WebDAV

!!! success Bonne pratique
Créer un fichier de conf par site, ne pas oublier de changer le port, pour ne pas avoir plusieurs configurations avec le même port.
!!!

- Ici on va éditer directement le fichier défaut, mais on pourrait très bien le copier et renommer:

```
sudo cp /etc/nginx/sites-enabled/default /etc/nginx/sites-enabled/site.conf
sudo rm /etc/nginx/sites-enabled/default
```

---

## :memo: Éditer le fichier de configuration

```
sudo nano /etc/nginx/sites-enabled/site.conf
```

``` # /etc/nginx/sites-enabled/default
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}

        location ^~ /webdav {
          auth_basic          "realm_name";
          auth_basic_user_file /var/www/.auth.allow;
          alias /var/www/html;
          autoindex on;
          autoindex_exact_size on;
          autoindex_localtime on;
          index file.html;
          dav_methods PUT DELETE MKCOL COPY MOVE;
          dav_ext_methods PROPFIND OPTIONS;
          dav_access user:rw;
          client_body_temp_path /var/www/tmp;
          client_max_body_size 0;
          create_full_put_path on;

        } 
}
```

:icon-chevron-right: `Ligne 1-3` Le serveur écoute sur le port 80.  

:icon-chevron-right: `Ligne 5` Définit le chemin de la racine du site internet (ou se trouve votre index.html, par exemple).  

:icon-chevron-right: `Ligne 18` `location ^~ /webdav {` dit que pour atteindre `/var/www/html` je rentre `l'ip ou le nom de domaine de ma machine` + `/webdav` = `http://172.16.30.30/webdav` par exemple.  

:icon-chevron-right: `Ligne 20` Définit le chemin du fichier d'authentification.  

---

## :heavy_plus_sign: Création du fichier d'authentification

:icon-upload: Passage en root

```
su -
```

:icon-git-pull-request-draft: Remplacez `user` par votre nom d'utilisateur

```
echo -n 'user:' | tee -a /var/www/.auth.allow 
```

:icon-key-asterisk: Définir le mot de passe de l'utilisateur:  
Rentrez votre mot de passe, confirmez, il s'affichera sous forme de hash.

```
openssl passwd -apr1 | tee -a /var/www/.auth.allow
```

``` openssl passwd -apr1 | tee -a /var/www/.auth.allow
Password:
Verifying - Password:
$apr1$t.VOQfZL$bHLajKSa1gA34tgAVWA2l/
```

!!! success
Vérifier la configuration de l'utilisateur:
```
cat /var/www/.auth.allow
```

``` cat /var/www/.auth.allow
user_name:$apr1$9WppBYUH$L4S3jfDRXqfcAJ1mD93KD/
```
!!!

---

## :books: Définition des autorisations du fichier d'authentification

```
chown root:www-data /var/www/.auth.allow && chmod 640 /var/www/.auth.allow
```

---

## :chart: Activer la compression avec gzip (économise la bande passante)

**La compression gzip est OPTIONNELLE et est déconseillé si le site utilise https.**

```
sed -i '/gzip_/ s/#\ //g' /etc/nginx/nginx.conf
```

---

## :arrows_counterclockwise: Tester et redémarrer nginx

```
nginx -t && systemctl restart nginx
```

!!! success
Vous pouvez vous connecter au partage webdav (/var/www/html) en utilisant le lien (http://IP_serveur/webdav)
!!!

==-