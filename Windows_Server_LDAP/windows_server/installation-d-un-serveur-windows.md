---
icon: download
image: images/ws.webp
description: Installation d'un serveur windows.
categories: [Windows server]
date: 2021-11-12
tags: [Windows server]
visibility: public
order: 40
---

# :icon-download: Installation d'un serveur windows

:icon-info: Un Serveur windows permet à une entreprise de gérer un ou plusieurs rôles.  

:icon-chevron-right: Le `rôle` est un terme propre a windows server qui désigne une fonctionnalité ajoutée après son installation.  

=== Voici quelques rôles fréquemments utilisés:

 AD DS | DNS | DHCP
--- | --- | ---
Active directory (contrôleur de domaine) | Résolution de noms | Distribution de la configuration réseau (IP, DNS, Gateway, PXE...)  

||| Services de stockage (DFS)
Liens logiques de stockage et réplication
||| WSUS
déploiement de mises à jour
||| WDS
déploiement d'images systèmes
||| Hyper-V
Virtualisation de services
||| RDS
Prise de main à distance
|||

===

---

:icon-download: Télécharger l'ISO Windows Server (peux importe la version, ici 2016):
[!file icon="rocket" text="Télécharger l'ISO"](https://store7.gofile.io/download/793bfd9b-f35a-484b-a713-3875928a2e7f/W2K16.iso)


=== Il est conseillé la configuration minimum suivante:

||| CPU
2 Coeurs
||| RAM
2GB
||| DD
40GB
|||

===

!!!
Pour un serveur WSUS, il est recommandé de mettre le maximum de ressources possibles à votre machine.  
Ce rôle est très gourmand (Téléchargement et déploiement de grande quantités de données).
!!!

---

## :1234: Processus d'installation  

![Choisissez votre langue et format clavier.](images/install-ws/1.webp)

![Installer l'OS.](images/install-ws/2.webp)

![Entrez une clé d'activation si vous en possédez une.](images/install-ws/3.webp)

![Installation avec `expérience utilisateur` (environnement graphique).](images/install-ws/4.webp)

![Accpetez les termes et la licence](images/install-ws/5.webp)

![Choisissez `Installer uniquement Windows (avancé)`](images/install-ws/6.webp)

![Partitionnez le dique à vos besoins.](images/install-ws/7.webp)

![Entrez un mot de passe fort pour l'Administrateur du serveur](images/install-ws/8.webp)

---
























