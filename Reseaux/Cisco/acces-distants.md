---
description: Les accès distants sur équipements CISCO.
categories: [Réseaux, Cisco]
date: 2022-09-14
tags: [Networks, config, tuto, Cisco]
order: 45
---

# :outbox_tray: Accès distants

!!! success Bonne pratique
Nommer chaque équipement avec une nomenclature réfléchie, c'est gagner du temps.
```
SW1(config)#hostname SW1
```
!!!

## :unlock: Telnet

Telnet est le moyen le plus rapide à configurer, mais n’est pas sécurisé.
Il est nécessaire d’attribuer un mot de passe :

```
SW1(config)#enable secret MON_MDP
```

Pour atteindre votre équipement en telnet, il faut un vlan actif avec une adresse IP.

!!! success Bonne pratique
IL est conseillé d'utiliser le vlan 99, pour simplifier l'administration des équipements.
!!!

```
SW1(config)#vlan 99
SW1(config-vlan)#exit
SW1(config)#int vlan 99
SW1(config-if)#

%LINK-5-CHANGED: Interface Vlan99, changed state to up
SW1(config-if)#ip address 192.168.1.2 255.255.255.0
SW1(config-if)#exit
SW1(config)#ip default-gateway 192.168.1.254  /*Passerelle LAN
```

- Activer telnet :

``` SW1(config)# 
line vty 0 1
```

!!! primary
`0` est l'`ID` de la ligne.
`1` est le nombre maximum de connexions simultanées.
!!!

Configuration du mot de passe de la ligne:

```
SW1(config-line)#password MON_MDP
SW1(config-line)#login
```

----

### :bookmark_tabs: Résumé des commandes telnet

Une fois votre `vlan 99` créé et adressé, que votre `gateway` est bien définie;  
vous pouvez copier/coller directement ces lignes (en adaptant votre configuration), dans votre terminal:

```
conf t
int vlan 99
ip address 192.168.1.2 255.255.255.0
no shut
exit
ip default-gateway 192.168.1.1
line vty 0 1
password MON_MDP
login
```

---

## :closed_lock_with_key: SSH

Le moyen le plus sécurisé, il ajoute une couche de chiffrement.
Sa configuration, est dans un premier temps, similaire au telnet:

```
SW1(config)#int vlan 99
SW1(config-if)#ip address 192.168.1.2 255.255.255.0
SW1(config-if)#no shut
SW1(config-if)#exit
SW1(config)#ip default-gateway 192.168.1.1  /*Passerelle LAN
```

- Il est nécessaire de renseigner un nom de domaine :

```
SW1(config)#ip domain-name 1234.com
```

- Génération des clés de chiffrement RSA :

```
SW1(config)#crypto key generate rsa
The name for the keys will be: SW1.1234.com
Choose the size of the key modulus in the range of 360 to 2048 for your
General Purpose Keys. Choosing a key modulus greater than 512 may take
a few minutes.

How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable...[OK]
```

!!! Warning
Pour éviter les failles de sécurité répandues avec SSH v1, passez en version 2 :
```
SW1(config)#ip ssh version 2
```
!!!

- Configuration d'un login/mot de passe :

```
SW1(config)#username admin password MON_MDP
```

- Configuration de la ligne 0, pour déclarer que seul 1 utilisateur sur le protocole SSH sera autorisé :

```
SW1(config)#line vty 0 1
SW1(config-line)#login
SW1(config-line)#transport input ssh
```

---

### :bookmark_tabs: Résumé des commandes SSH

Copier/coller directement ces lignes (en adaptant votre configuration), dans votre terminal :

```
conf t
int vlan 99
ip address 192.168.1.2 255.255.255.0
no shut
exit
ip default-gateway 192.168.1.1
ip domain-name 1234.com
crypto key generate rsa
2048
ip ssh version 2
username admin password MON_MDP
line vty 0 1
login
transport input ssh
```

----




























