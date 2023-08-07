---
icon: meter
image: images/quota/quota.webp
description: Appliquer un quota sur des repertoires partagés en réseau.
categories: [Active directory]
date: 2020-11-30
tags: [Active directory, AD, AD DS, domain, domain controller, controleur de domaine, share, permissions, quota]
order: 48
---

# :icon-meter: Appliquer un quota sur des répertoires partagés en réseau

![](images/quota/quota.webp)

---

## :small_blue_diamond: Contexte

Vous devez mettre en place des restrictions pour plusieurs ensembles d'utilisateurs.
En se basant sur la configuration du précédent tutoriel:

[!ref Active Directory multi-sites, partages réseaux et gestion des droits utilisateur](active-directory-tp-uo-ou-gg-gl-partages-et-droits-utilisateurs.md)

Nous allons appliquer des "quotas" aux partages de dossiers sur le serveur de Chassignieu par exemple.

Voici l'arborescence des dossiers partagés:

```
▶️SRV-CHA
├── 📁DATA-CHA
│   ├── 📁Tech-Commun
│   ├── 📁Adm-Commun
│   ├── 📁Adm-Chassignieu
│   ├── 📁Tech-Chassignieu
│   └── 📁Direction
│       ├── 📁Investissements
│       └── 📁RH
├── 📁PROFILS-CHA
└── 📁DBASE-CHA
```

Voici les exigences de l'entreprise:

Techniciens | commun | Administratifs | commun | Techniciens de Chassignieu | Administratifs de Chassignieu | Direction
--- | --- | --- | --- | --- | --- | ---
1Go | 500Mo | 1Go | 500Mo | 2Go | 4Go | 4Go

:icon-arrow-right: Profils  
:icon-arrow-right: utilisateurs  
:icon-arrow-right: Dossier de base  

---

## :heavy_plus_sign: Ajout du rôle "Gestionnaire de ressources du serveur de fichiers"

![](images/quota/q1.webp)

![](images/quota/q2.webp)

![](images/quota/q3.webp)

![](images/quota/q4.webp)

![](images/quota/q5.webp)

![](images/quota/q6.webp)

![](images/quota/q7.webp)

![](images/quota/q8.webp)

![](images/quota/q9.webp)

![](images/quota/q10.webp)

---

## :newspaper: Création d'un modèle de quota

![](images/quota/q11.webp)

![Liste des modèles par défaut](images/quota/q12.webp)

![](images/quota/q13.webp)

![](images/quota/q14.webp)

---

## :memo: Appliquation des quotas


![](images/quota/q15.webp)

![](images/quota/q16.webp)

![](images/quota/q17.webp)

---

## :lock: Bonus: Appliquer des filtres

![](images/quota/q18.webp)

![Séléctionner le type de fichier que vous voulez bloquer](images/quota/q19.webp)

![](images/quota/q20.webp)

---

















