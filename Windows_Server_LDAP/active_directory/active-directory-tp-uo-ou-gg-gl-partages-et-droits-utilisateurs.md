---
description: Active directory, installation, contrôleur de domaine, partages et autorisations.
image: images/ad.webp
icon: codespaces
categories: [Active directory]
date: 2021-12-20
tags: [Windows server, Active directory, AD, DS, domain, share, permissions, domain controler, controleur de domaine]
order: 49
---

# :key: Active Directory multi-sites, partages réseaux et gestion des droits utilisateur

![](images/ad.webp)

---

## :receipt: Contexte

Une entreprise possède 3 sites, chacun d'entre-eux ont 1 serveur windows:  
- SRV-CHA (AD) sur Chassignieu (siège de l'entreprise)  
- SRV-VIR sur Virieu  
- SRV-BLA sur Blandin  

Chaque site contient 2 services:  
- Administratifs  
- Techniciens  

Et le siège en contient 3:
- Direction  
- Administratifs  
- Techniciens  

---

### :card_file_box: Organisation des Unités d'organisation (UO/OU)

```
▶️SRV-CHA
├── 👨‍👦‍👦Direction
│   ├── GG-CHA-DIR
│   ├── 🧑PDG
│   └── 🧑RH
├── 👨‍👦‍👦Administratifs
│   ├── GG-CHA-ADM
│   └── 🧑ADM1C
└── 👨‍👦‍👦Techniciens
    ├── GG-CHA-TECH
    └── 🧑TECH1C

▶️SRV-VIR
├── 👨‍👦‍👦Administratifs
│   ├── GG-VIR-ADM
│   └── 🧑ADM1V
└── 👨‍👦‍👦Techniciens
    ├── GG-VIR-TECH
    └── 🧑TECH1V

▶️SRV-BLA
├── 👨‍👦‍👦Administratifs
│   ├── GG-BLA-ADM
│   └── 🧑ADM1B
└── 👨‍👦‍👦Techniciens
    ├── GG-BLA-TECH
    └── 🧑TECH1B
```
Les `GG (Groupes Globaux)` font la liaison entre les utilisateurs et les GL.   
Ils regroupent tous les utilisateurs d'un même service.  
Chaque utilisateur appartient au GG de son service.  

---

### :open_file_folder: Partages et permissions

```
▶️SRV-CHA
├── 📁DATA-CHA
│   ├── 📁Tech-Commun
│   │   └── 🔓GL-SRV-CHA-DATA-TECH-COMMUN-CT
│   ├── 📁Adm-Commun
│   │   ├── 🔓GL-SRV-CHA-DATA-TECH-COMMUN-CT
│   │   └── 🔒GL-SRV-CHA-DATA-ADM-COMMUN-L
│   ├── 📁Adm-Chassignieu
│   │   ├── 🔓GL-SRV-CHA-DATA-ADM-CHASSIGNIEU-CT
│   │   └── 🔒GL-SRV-CHA-DATA-ADM-CHASSIGNIEU-L
│   ├── 📁Tech-Chassignieu
│   │   └── 🔓GL-SRV-CHA-DATA-TECH-CHASSIGNIEU-CT
│   └── 📁Direction | 🔓GL-SRV-CHA-DATA-DIRECTION-CT
│       ├── 📁Investissements
│       └── 📁RH
├── 📁PROFILS-CHA
└── 📁DBASE-CHA

▶️SRV-VIR
├── 📁DATA-VIR
│   ├── 📁Adm-Virieu
│   │   ├── 🔓GL-SRV-VIR-DATA-ADM-VIRIEU-CT
│   │   └── 🔒GL-SRV-CHA-DATA-ADM-VIRIEU-L
│   └── 📁Tech-Virieu
│       └── 🔓GL-SRV-VIR-DATA-TECH-VIRIEU-CT
├── 📁PROFILS-VIR
└── 📁DBASE-VIR

▶️SRV-BLA
├── 📁DATA-BLA
│   ├── 📁Adm-Blandin
│   │   ├── 🔓GL-SRV-BLA-DATA-ADM-BLANDIN-CT
│   │   └── 🔒GL-SRV-CHA-DATA-ADM-BLANDIN-L
│   └── 📁Tech-Blandin
│       └── 🔓GL-SRV-BLA-DATA-TECH-BLANDIN-CT
├── 📁PROFILS-BLA
└── 📁DBASE-BLA
```

Nous allons nous baser sur la topologie ci-dessus, pour l'ensemble de ce TP.

Les étapes détaillées ci-dessous vous indique la démarche à suivre pour créer chacun des éléments.  

---

## :heavy_plus_sign: Création d'une OU

`OU` Organisation Unit en anglais ou `UO` Unité d'organisation en francais est un élément dans l'active directory, qui nous permet de gérer un ensemble d'éléments.
Sur le schéma ci-dessus, nous pouvons remarquer qu'il ya des "sous Unités d'organisation":  

Par exemple: L'utilisateur: PDG se trouve dans `SRV-CHA > Direction`  
Donc dans la sous OU `Direction` qui dépend de `SRV-CHA`  


- RDV sur le serveur `SRV-CHA` (active directory):  

![](images/ad_multisites/util.webp)

![](images/ad_multisites/1.webp)

![Décochez la case `Protéger le conteneur contre une supression accidentelle`](images/ad_multisites/2.webp)

---

## :heavy_plus_sign: Création d'un GG

Un `Groupe global` permet de rassembler un ou plusieurs utilisateurs.  

![](images/ad_multisites/3.webp)

![](images/ad_multisites/4.webp)

---

## :heavy_plus_sign: Création d'un Utilisateur

![](images/ad_multisites/5.webp)

![](images/ad_multisites/6.webp)

![Définir un mot de passe qui respècte votre stratégie de sécurité](images/ad_multisites/7.webp)

![](images/ad_multisites/8.webp)

---

## :heavy_plus_sign: Création d'un GL

Les `GL (Groupes de Domaine Local)` appliquent les droits aux ressources.  

`CT` Contrôle total  
`L` Lecture  

![](images/ad_multisites/22.webp)

![](images/ad_multisites/23.webp)

![](images/ad_multisites/24.webp)

![](images/ad_multisites/25.webp)

---

## :heavy_plus_sign: Création du repertoire partagé "DATA"

![](images/ad_multisites/17.webp)

![](images/ad_multisites/18.webp)

![](images/ad_multisites/19.webp)

![](images/ad_multisites/20.webp)

![](images/ad_multisites/21.webp)

![C'est dans ces sous-dossiers que les GL seront appliqués](images/ad_multisites/22.webp)

### :heavy_check_mark: Activer l'énumération basée sur l'accès

:icon-info: Activer l'énumération permet d'afficher seulement les dossiers auxquels l'utilisateur aura un accès.

[![](images/ad_multisites/x1.webp)](images/ad_multisites/x1.webp)

[![](images/ad_multisites/x2.webp)](images/ad_multisites/x2.webp)

[![](images/ad_multisites/x3.webp)](images/ad_multisites/x3.webp)

![Cocher `Activer l'énumération basée sur l'accès`](images/ad_multisites/x3.webp)

---

### :link: Lier un utilisateur au GG 

![](images/ad_multisites/26.webp)

![](images/ad_multisites/27.webp)

![](images/ad_multisites/28.webp)

---

### :link: Lier un GG au GL

![](images/ad_multisites/29.webp)

![](images/ad_multisites/30.webp)

![](images/ad_multisites/31.webp)

![](images/ad_multisites/32.webp)

---

### :bookmark_tabs: Appliquer un GL au repertoire partagé

==- :icon-diff-renamed: Permet d'appliquer les permissions au chemin partagé.

![](images/ad_multisites/33.webp)

![](images/ad_multisites/34.webp)

![](images/ad_multisites/35.webp)

![N'autoriser l'accès qu'aux membres du GG-Direction](images/ad_multisites/36.webp)

![](images/ad_multisites/37.webp)

![](images/ad_multisites/38.webp)

![](images/ad_multisites/39.webp)

![](images/ad_multisites/40.webp)

==-

---

## :part_alternation_mark: Dossier de base

:icon-info: Le dossier de base est simplement un repertoire vide pour chaque utilisateur, ou il peut y stocker ses documents par exemple.  
Ce dossier est dans un chemin partagé.

==- :icon-diff-added: Créer le partage réseau

![](images/ad_multisites/9.webp)

![](images/ad_multisites/10.webp)

![Le symbole `$` permet de cacher le repertoire](images/ad_multisites/11.webp) 

![](images/ad_multisites/12.webp)

![](images/ad_multisites/13.webp)

![](images/ad_multisites/14.webp)

==-

==- :icon-duplicate: Lier le dossier de base à l'utilisateur

![](images/ad_multisites/15.webp)

![Coller le chemin du partage réseau.  
`%USERNAME%` permet de créer un dossier qui porte le nom de chaque utilisateur](images/ad_multisites/16.webp)

==-

---

## :outbox_tray: Montage des lecteurs réseaux

:icon-sort-asc: Chaque utilisateur va monter 3 lecteurs réseaux sur sa session automatiquement (le commun, cloisonné et le dossier de base, dépendant du lieu ou il se trouve), à l'aide du script qui lui est assigné:  

=== - Chassignieu:

```
net use Y: \\SRV-CHA\DATA-CHA$
```

!!!
N'en contient que 2 puisque le dossier `Commun` et `Cloisonné` sont tous les deux dans `DATA-CHA`
!!!

===


=== - Virieu:

```
net use Y: \\SRV-CHA\DATA-CHA$
net use Z: \\SRV-VIR\DATA-VIR$
```

===


=== - Blandin:

```
net use Y: \\SRV-CHA\DATA-CHA$
net use Z: \\SRV-BLA\DATA-BLA$
```

===

:icon-chevron-right: Prennons l'exmeple d'un utilisateur sur Blandin:

`X:` contiendra le repertoire partagé `\\SRV-BLA\DBASE-BLA$\NOM_UTILISATEUR` 

`Y:` contiendra le repertoire commun partagé `\\SRV-CHA\DATA-CHA$` (l'utilisateur ne pourra voir que les dossiers ou il aura un accès (minimum en lécture) du partage commun).  

`Z:` contiendra le repertoire partagé `\\SRV-BLA\DATA-BLA$` si je suis à Blandin (l'utilisateur ne pourra voir que les dossiers ou il aura un accès (minimum en lécture), cloisonnement par site).  

==- :ballot_box_with_ballot: Création des scripts

![](images/ad_multisites/41.webp)

![Cocher `Extension de noms de fichiers`](images/ad_multisites/42.webp)

![](images/ad_multisites/43.webp)

![](images/ad_multisites/44.webp)

==-

==- :link: Lier le script à l'utilisateur

![](images/ad_multisites/45.webp)

![](images/ad_multisites/46.webp)

==-

---

## :curly_loop: Profils itinérants

Les Profils itinérants, facilitent le changement de machine ou de locaux.  
Le profil utilisateur est stocké de manière distante sur un repertoire partagé.  

==- :ballot_box_with_ballot: Créer le repertoire partagé

![](images/ad_multisites/47.webp)

![](images/ad_multisites/48.webp)

![](images/ad_multisites/49.webp)

![](images/ad_multisites/50.webp)

![](images/ad_multisites/51.webp)

==-

==- :link: Lier le profil à l'urilisateur

![](images/ad_multisites/52.webp)

![](images/ad_multisites/53.webp)

==-

---

## :white_check_mark: Tester un utilisateur du domaine

!!!
Il est nécessaire d'être administrateur local de la machine pour rejoindre le domaine.
!!!

---















