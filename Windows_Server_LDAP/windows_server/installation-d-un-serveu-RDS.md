---
icon: download
image: images/ws.webp
description: Installation d'un serveur Remote Desktop Protocol.
categories: [Windows server]
date: 2021-11-22
tags: [Windows server, serveur, rds, remote, desktop]
visibility: public
order: 39
---

# :icon-download: Installation d'un serveur RDS

---

## :heavy_plus_sign: Ajouter les rôles nécessaires

---

![](images/rds/1.webp)

---

![](images/rds/2.webp)

---

![](images/rds/3.webp)

---

![Le Démarrage rapide installe tous les rôles nécessaires au serveur RDS sur celui-ci (BROKER, GATEWAY, RDS).](images/rds/4.webp)

---

![](images/rds/5.webp)

---

![](images/rds/6.webp)

---

![](images/rds/7.webp)

---

![Le serveur va redémarrer automatiquement](images/rds/8.webp)

---

![](images/rds/9.webp)

---

![](images/rds/10.webp)

---

## :x: Désactiver l'authentification NLA

Permet de ne pas avoir le message d'erreur de certificat.

---

![](images/rds/11.webp)

---

![](images/rds/12.webp)

---

## :ballot_box_with_check: Connexion sur le client RDS

Vous pouvez maintenant tester votre serveur et vous connecter en bureau à disatance avec votre client.

---

![`Win + R: Menu Exécuter > mstsc`](images/rds/13.webp)

---

![](images/rds/14.webp)

1. Renseigner l'adresse IP du serveur RDS  
2. Renseignler le nom de domaine \ nom d'utilisateur


---

![1. Renseigner le mot de passe de l'utilisateur](images/rds/15.webp)



---

![](images/rds/16.webp)

---

![La session est bien connectée.](images/rds/17.webp)









































