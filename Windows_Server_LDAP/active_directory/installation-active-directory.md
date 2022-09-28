---
icon: download
description: Installation d'un serveur windows avec le rôle AD DS (Controleur de domaine).
image: images/adins.webp
categories: [Active directory]
date: 2021-11-12
tags: [Windows server, Active directory, AD, AD DS, domain controler, domain, controleur de domaine]
visibility: public
order: 50
---

# :icon-download: Installation d'un serveur active directory

![](images/adins.webp)

Un Active Directory ou controleur de domaine, c'est un rôle ou une fonctionnalité ajoutée au serveur windows après sont installation.


Si vous n'avez pas encore installer votre serveur windows vous pouvez trouver un tutoriel étape par étape ici:

[!ref Installer un serveur windows server](https://windows.contactit.fr/windows-server/install-ws.md)


---

## :exclamation: Fixer l'adresse IP et le nom de la machine

![`Win + R` (menu éxécuter) et entrez: `ncpa.cpl`  
(ouvre directement les paramètres des adaptateurs réseaux)](images/install-ad/1.webp)

---

![Double-clickez sur `Protocol Internet version 4 (TCP/IPv4)`](images/install-ad/2.webp)

---

![](images/install-ad/3.webp)

`Adresse IP:` `IP_AD`  
`Masque de sous-réseau:` `MASQUE_AD`  
`Passerelle par défaut:` `IP_PASSERELLE`  

:icon-info: Pour ce qui est du DNS celui-ci se mettra tout seul en loopback.
Quand le serveur windows serveur est promu contrôleur de domaine, il installe le rôle DNS automatiquement et devient donc, aussi son propre DNS

---

![`Serveur Local > Nom de l'ordinateur`](images/install-ad/4.webp)

---

![`Nom de l'ordinateur > Modifier...`](images/install-ad/5.webp)

---

![Renommer votre machine, `Ok` puis redémarrer](images/install-ad/6.webp)

---

## Ajouter la fonctionnalité "AD DS"

!!!
Celle-ci ajoutera automatiquement la fonctionnalité DNS, puique l'AD en a besoin pour fonctionner.
!!!

![](images/install-ad/7.webp)

---

![](images/install-ad/8.webp)

---

![](images/install-ad/9.webp)

---

## :ballot_box_with_ballot: Promouvoir le serveur en contrôleur de domaine

!!!
Lors de la promulugation du serveur en contrôleur de domaine, celui-ci retire tous les utilisateurs locaux.
!!!

![](images/install-ad/10.webp)

---

![](images/install-ad/11.webp)

---

![Il est nécessaire de spécifier un mot de passe](images/install-ad/12.webp)

---

![](images/install-ad/13.webp)

---

![](images/install-ad/14.webp)

---

![](images/install-ad/15.webp)

---

![](images/install-ad/16.webp)

---

![Installation du contrôleur de domaine](images/install-ad/17.webp)

---

![Le serveur va redemarrer.](images/install-ad/18.webp)

---

![Le serveur est bien dans le domaine.](images/install-ad/19.webp)

---













